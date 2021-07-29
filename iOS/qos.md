# Global DispatchQueue 의 Qos 에는 어떤 종류가 있는지, 각각 어떤 의미인지 설명하시오.

## 도움이 된 글
* [Prioritize Work with Quality of Service Classes - Zedd](https://zeddios.tistory.com/521)
* [Concurrency Programming Guide - DispatchQueue](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationQueues/OperationQueues.html#//apple_ref/doc/uid/TP40008091-CH102-SW1)
* [Energy Efficiency Guide for iOS Apps - Prioritize Work with Quality of Service Classes](https://developer.apple.com/library/archive/documentation/Performance/Conceptual/EnergyGuide-iOS/PrioritizeWorkWithQoS.html#//apple_ref/doc/uid/TP40015243-CH39-SW1)

## Answer

```xml
priority가 높은 작업은 우선순위가 낮은 작업보다 더 빨리 수행되고 리소스가 많으므로 일반적으로 priority가 낮은 작업보다 더 많은 에너지가 필요합니다. 앱이 수행하는 작업에 대해 적절한 QoS클래스를 정확하게 지정하면, 앱이 에너지 효율적일뿐만 아니라, 반응적임을 보장 할 수 있습니다.
```

### 종류

* 4개의 primary QoS class
    * User-interactive 
    * User-initiated 
    * Utility 
    * Background 
* 2개의 Special Quality of Service Classes  
    > 대부분의 경우 이 두개의 클래스에 노출될 일은 없다고 한다.
    * Default   
    * Unspecified

| Type | Qos Class | Type of work and focus of QoS | Duration of work to be performed |
| :--- | :------- | :---------------------------- | :------------------------------- |
| Primary | User-interactive  | main thread에서 동작하거나 UI를 새로고침하거나 애니메이션을 수행하는 등 사용자와 상호작용 하는 작업입니다. 만약 작업이 신속하게 수행되지 않으면 UI가 멈춰보일 수 있습니다. 반응성과 성능에 중점을 둡니다. | 작업 시간은 거의 즉각적입니다. |
| Primary | User-initiated    | 사용자가 시작했던 작업 그리고 즉각적인 결과가 요구되는 작업입니다. 저장된 문서를 열거나 사용자가 UI에 존재하는 어떤 것을 클릭하여 액션이 발생하는 등의 작업이 될 수 있습니다. UI를 계속해서 진행되게 하기 위해서는 그 작업이 필요합니다.  반응성과 성능에 중점을 둡니다. | 작업은 몇 초 이하와 같이 거의 즉각적입니다. |
| Primary | Utility           | 완료까지 약간의 시간이 걸리거나 즉각적인 결과가 요구되지 않는 작업입니다. 데이터를 다운로드하거나 내보내는 등의 작업이 될 수 있습니다. Utility 작업들은 일반적으로 사용자에게 보여지는 진행 막대를 가지고 있습니다. 반응성, 성능 및 에너지 효율성 간에 균형을 유지하는 데 중점을 둡니다. | 작업은 몇 초에서 몇 분이 소요됩니다. |
| Primary | Background        | Background에서 수행되며 사용자에게 보여지지 않는 작업입니다. 이를테면 indexing, 동기화 및 백업 등의. 에너지 효율성에 중점을 둡니다. | 작업에 상당한 시간이 소요됩니다. 수 분에서 수 시간. |
| Special | Default           | 이 QoS의 priority level은 user-initiated와 utility사이에 있습니다. 이 QoS는 개발자가 작업을 분류하는데 사용하기 위한 것이 아닙니다. QoS정보가 할당되지 않은 작업은 Default로 처리되며 GCD global queue는 이 level(default)에서 실행됩니다. | - |
| Special | Unspecified       | 이는 QoS정보가 없음을 나타내며, 환경 QoS(environmental QoS)를 추론해야 한다는 단서를 시스템에 제공합니다. 쓰레드가 기존(legacy) API를 사용하는 경우, Unspecified QoS를 사용할 수 있으며, 이경우 쓰레드가 QoS를 벗어날 수 있습니다. | - |