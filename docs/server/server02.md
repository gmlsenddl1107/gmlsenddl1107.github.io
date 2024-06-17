---
layout: default
title: LCEL (Langchain) - 의문1. 어떻게 구현되어 있는건지
parent: Server
nav_order: 2
---
이번 글에서는 LCEL에 대한 아래 의문점 중 1번에 대해 다룰 예정이다.

**의문1. 어떻게 구현되어 있는건지**  
runnable 프로토콜을 구현하여 | 로 chain을 구성하였다고 하는데 도대체 어떻게 동작하고 어떻게 구현을 한지 감이 잡히지 않았다.

```
chain = prompt | model | output_parser 
```

도대체 어떻게 구현되었길래 | 기호로 chain을 만들었다는 것일까?

# 코드 까보기

lcel은 runnable들을 가지고 chain을 구성하는 방식으로 되어있다.  
promptTempate 등도 결국 Runnable를 상속하고 있다.

## Runnable

각각 promptTemplate, model등 chaining하여 사용하는 componet 하나 하나 단위로 사용되며. 내부를 확인해 보면 coerce\_to\_runnable를 통해 type을 확인하여 generator나 callable, dict을 전부 runnable 객체로 바꾼 후에 사용하게 된다.

**runnable 주요 내용**

-   input, output 등 스키마 정보
-   invoke 함수사용하여 통해 runnable을 실행한다.(즉, 각각의 runnable는 invoke가 구현되어있다.)
-   \_\_or\_\_ 함수 통한 chaining

runnable 코드의 \_\_or\_\_확인해보면 아래와 같다.  
python에는 매직 메소드가 있는데 그 중 \_\_or\_\_를 custom하게 구현해 놓은 것을 확인할 수 있다.

```
    def __or__(
        self,
        other: Union[
            Runnable[Any, Other],
            Callable[[Any], Other],
            Callable[[Iterator[Any]], Iterator[Other]],
            Mapping[str, Union[Runnable[Any, Other], Callable[[Any], Other], Any]],
        ],
    ) -> RunnableSerializable[Input, Other]:
        """Compose this runnable with another object to create a RunnableSequence."""
        return RunnableSequence(self, coerce_to_runnable(other))
```

other 는 | 기준 오른쪽에 위치한 아이로 \_\_or\_\_이 호출된 아이와 함께 합쳐져서 RunnableSequence로 들어가게 된다.

### Runnable의 \_\_or\_\_ 의 chaining 과정 해석

```
chain = prompt | model  
```

위 예시 기준으로는 "Prompt | model" 이 호출되면서 other에는 model이 들어가게되고 Prompt와 model이 chainning된 RunnableSequence를 return하게 된다.( Prompt는 runnable을 상속한 객체 )

\*\*coerce\_to\_runnable 함수  
: type을 확인하여 generator나 callable, dict을 전부 runnable 객체로 바꾸어 주는 역할을 해준다.

## RunnableSequence, RunnableParallel  등

Runnable들을 실제로 chaining하는 역할을 한다.  
기본적으로 "|"로 chaining을 하게 되면 기본적으로 RunnableSequence 객체가 된다.  
RunnableSequence가 chaining한 runnable 객체를 구동하는 역할을 하는 메인인 것으로 확인된다.

### RunnableSequence

RunnableSequence의 init 함수를 보면 아래와 같다.

```
def __init__(
        self,
        *steps: RunnableLike,
        name: Optional[str] = None,
        first: Optional[Runnable[Any, Any]] = None,
        middle: Optional[List[Runnable[Any, Any]]] = None,
        last: Optional[Runnable[Any, Any]] = None,
    ) -> None:
        """Create a new RunnableSequence.

        Args:
            steps: The steps to include in the sequence.
        """
        steps_flat: List[Runnable] = []
        if not steps:
            if first is not None and last is not None:
                steps_flat = [first] + (middle or []) + [last]
        for step in steps:
            if isinstance(step, RunnableSequence):
                steps_flat.extend(step.steps)
            else:
                steps_flat.append(coerce_to_runnable(step))
        if len(steps_flat) < 2:
            raise ValueError(
                f"RunnableSequence must have at least 2 steps, got {len(steps_flat)}"
            )
        super().__init__(  # type: ignore[call-arg]
            first=steps_flat[0],
            middle=list(steps_flat[1:-1]),
            last=steps_flat[-1],
            name=name,
        )
```

RunnableSequence는 steps\_flat List에 runnable 객체를 담아 실행 순서 List를 가지고 있는다.

### Runnable의 \_\_or\_\_ 의 chaining 과정에서의 RunnableSequence을 다시 확인해보자

```
chain = prompt | model | output_parser 
```

"|"로 chaining을 하게 되면 기본적으로 RunnableSequence 객체가 된다.  
즉, 위 코드의 chain 변수는 RunnableSequence 객체이다.

Prompt는 runnable을 상속한 객체이고 "Prompt | model" 이 호출되면서 other에는 model이 들어가게되고 Prompt와 model이 chainning된 RunnableSequence를 return하게 된다. 여기서는 return된 RunnableSequence를 A 라고 하자.  
그러면 그 다음으로는 A | output\_parser가 되는데 other에 output\_parser가 들어가고 prompt, model, output\_parser가 chaining 된 RunnableSequence를 return하게 된다.(RunnableSequence은 runnable을 상속하고 있다.)

RunnableSequence의 \_\_or\_\_함수는 Runnable의 \_\_or\_\_ 함수와 비슷하며  많은 runnable들을 순서에 맞게 처리할 수 있게 first,middle,last로 구분해놓고있다.   
아래는 RunnableSequence의 \_\_or\_\_함수이다.

```
 def __or__(
        self,
        other: Union[
            Runnable[Any, Other],
            Callable[[Any], Other],
            Callable[[Iterator[Any]], Iterator[Other]],
            Mapping[str, Union[Runnable[Any, Other], Callable[[Any], Other], Any]],
        ],
    ) -> RunnableSerializable[Input, Other]:
        if isinstance(other, RunnableSequence):
            return RunnableSequence(
                self.first,
                *self.middle,
                self.last,
                other.first,
                *other.middle,
                other.last,
                name=self.name or other.name,
            )
        else:
            return RunnableSequence(
                self.first,
                *self.middle,
                self.last,
                coerce_to_runnable(other),
                name=self.name,
            )
```

위 함수로 보면 RunnableSequence를 생성하여 return하는것을 확인 할 수 있다.

### RunnableSequence 만든 chain의 실행방법(invoke)

RunnableSequence는 invoke 함수를 통해 runnable을 실행한다.  
아까 위에서 Runnable 객체도 전부 실행 코드인 invoke가 정의되어있는것을 확인했다.  
RunnableSequence는 invoke함수는 chainging 된 순서에 맞게 각 runnble을 invoke하게 된다.

RunnableSequence의 invoke함수는 아래와 같다

```
    def invoke(self, input: Input, config: Optional[RunnableConfig] = None) -> Output:
        from langchain_core.beta.runnables.context import config_with_context

        # setup callbacks and context
        config = config_with_context(ensure_config(config), self.steps)
        callback_manager = get_callback_manager_for_config(config)
        # start the root run
        run_manager = callback_manager.on_chain_start(
            dumpd(self),
            input,
            name=config.get("run_name") or self.get_name(),
            run_id=config.pop("run_id", None),
        )

        # invoke all steps in sequence
        try:
            for i, step in enumerate(self.steps):
                input = step.invoke(
                    input,
                    # mark each step as a child run
                    patch_config(
                        config, callbacks=run_manager.get_child(f"seq:step:{i+1}")
                    ),
                )
        # finish the root run
        except BaseException as e:
            run_manager.on_chain_error(e)
            raise
        else:
            run_manager.on_chain_end(input)
            return cast(Output, input)
```

input을 보면 이전 step의 output이 다음 step input으로 들어가는 것을 확인할 수 있다.
