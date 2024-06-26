### 작성자: 신지훈

### **Paging**

paging이란 process가 할당받은 메모리 공간을 일정한 page 단위로 나누어, 물리 메모리에서 연속되지 않는 서로 다른 위치에 저장하는 메모리 관리 기법이다.

나눈 메모리들의 크기는 모두 같으며, 조각낸 메모리들을 묶어서 물리 메모리에 넣고 필요할 때 조회하며, 필요가 없어지면 다시 프로세스에게 반환한다.

프로세스는 작업을 처리하기 위해 가상 주소를 CPU에게 전달하며, CPU는 받은 가상주소를 물리주소로 변환하고 데이터를 가져와야한다. 이때, 특정 데이터가 들어있는 데이터 조각 묶음을 계산하는 알고리즘이 바로 페이징이다.

> **[용어정리]** - 논리적 주소(logical address)란?
>
> process가 memory에 적재되기 위한 독자적인 주소 공간인 논리적 주소(logical address)가 생성된다. 논리적 주소는 각 process마다 독립적으로 할당되며, 0번지부터 시작된다.

> **[용어정리]** - 물리적 주소(physical address)란?
>
> 물리적 주소(physical address)는 process가 실제로 메모리에 적재되는 위치를 말한다.

> **[용어정리]** - 주소 바인딩(address binding)이란?
>
> CPU가 기계어 명령을 수행하기 위해 process**의 논리적 주소가 실제 물리적 메모리의 어느 위치에 매핑되는지 확인하는 과정**을 주소 바인딩(address binding)이라고 한다.

### Paging기법

paging 기법은 process의 메모리 공간을 동일한 크기의 page 단위로 나누어 물리적 메모리의 서로 다른 위치에 page들을 저장하는 메모리 관리 기법이다. paging 기법에서는 물리적 메모리를 **page와 같은 크기의 frame**으로 미리 나누어둔다. 프로세스마다 페이지 테이블이 존재한다.

page단위로 메모리 적재가 이뤄지기 때문에 미리 분할해두면 빠르게 메모리 할당이 이루어질 수 있다.

paging 기법에서는 주소 바인딩(address binding)을 위해 모든 프로세스가 각각의 주소 변환을 위한 페이지 테이블을 갖다.

page기법에서는 하나의 프로세스 내에서도 페이지 단위로 다른 물리적 메모리에 저장되기 때문에 주소 바인딩을 위해서는 별도의 페이지 테이블이 필요하다.

cf)페이지 테이블은 CPU의 PCB에 저장된다.

![1](https://github.com/EN-CS-STUDY/CS_STUDY/assets/81848426/3fa38b8c-b690-43ae-bd7f-3d36c948a08e)

page table을 이용한 주소 바인딩(1)

![2](https://github.com/EN-CS-STUDY/CS_STUDY/assets/81848426/7cb29acd-1ad9-4c56-92e0-01db4d73d784)

page table을 이용한 주소 바인딩(2)

**paging 기법 사용시 발생할 수 있는 메모리 단편화 문제가 있다.**

물리적 메모리 공간이 작은 조각으로 나눠져서 메모리가 충분히 존재함에도 할당이 불가능한 상태를 보고 메모리 단편화가 발생했다고 말한다.

paging 기법에서는 process의 논리적 주소 공간과 물리적 메모리가 같은 크기의 page 단위로 나누어지기 때문에 외부 단편화 문제가 발생하지 않는다.

하지만 process 주소 공간의 크기가 page 크기의 배수라는 보장이 없기 때문에, 프로세스의 주소 공간 중 가장 마지막에 위치한 page에서는 내부 단편화 문제가 발생할 가능성이 있다.

- **외부 단편화 문제**란 메모리상의 비어있는 공간의 크기가 작아서 빈 메모리 공간임에도 활용되지 못하는 문제
- **내부 단편화 문제**란 프로세스가 필요한 양보다 더 큰 메모리가 할당되어서 메모리 공간이 낭비되는 상황

---

### **Segmentation**

segmentation이란 process가 할당받은 메모리 공간을 논리적 의미 단위(segment)로 나누어, 연속되지 않는 물리 메모리 공간에 할당될 수 있도록 하는 메모리 관리 기법이다.

### Segmentation

segmentation 기법은 process가 할당받은 메모리 공간을 **논리적 의미 단위(segment)**로 나누어, 연속되지 않는 물리 메모리 공간에 할당될 수 있도록 하는 메모리 관리 기법이다.

일반적으로 process의 메모리 영역 중 Code, Data, Heap, Stack 등의 기능 단위로 segment를 정의하는 경우가 많다.

segmentation 기법에서는 주소 바인딩(address binding)을 위해 모든 프로세스가 각각의 주소 변환을 위한 segment table을 갖는다.

![segment는 그 크기가 균일하지 않기 때문에 논리적 주소가 <segment 번호, offset>으로 표현된다.
이 때 offset 값이 table의 limit 값보다 크면, 해당 segment를 넘어가므로 **segmentation fault 오류**가 발생한다.]![3](https://github.com/EN-CS-STUDY/CS_STUDY/assets/81848426/9ec0708f-4403-4714-8310-1782f804a22c)

segment는 그 크기가 균일하지 않기 때문에 논리적 주소가 <segment 번호, offset>으로 표현된다.
이 때 offset 값이 table의 limit 값보다 크면, 해당 segment를 넘어가므로 **segmentation fault 오류**가 발생한다.

**segmentation의 메모리 단편화가 있다.**

segmentation 기법에서 segment의 크기만큼 메모리를 할당하므로 내부 단편화 문제가 발생하지 않는다. 하지만 서로 다른 크기의 segment들이 메모리에 적재되고 제거되는 일이 반복되면, 외부 단편화 문제가 발생할 가능성이 있다.

**paging vs segmentation**

paging은 일정한 크기의 단위로 나누어 할당을 하는데, 이에 반해 segmentation은 code, data, heap, stack등의 기능(의미)단위로 물리 메모리에 할당을 하는 기법이다.

paging의 경우 내단편화의 문제가 발생할 수 있는데, 이에 반해 segmentation은 외부 단편화의 문제가 발생할 수 있다.

**paged segmentation 기법이란?**

paged segmentation이란 segmentation을 기본으로 하되 이를 다시 동일 크기의 page로 나누어 물리 메모리에 할당하는 메모리 관리 기법이다. 즉, 프로그램을 **의미 단위의 segment**로 나누고 개별 **segment의 크기를 page의 배수**가 되도록 하는 방법이다.

이를 통해 segmentation 기법에서 발생하는 **외부 단편화 문제를 해결**하고, 동시에 segment 단위로 process 간의 공유나 process 내의 접근 권한 보호가 이루어지도록 해서 paging 기법의 단점을 해결한다.

---

### 가상 메모리

가상 메모리(virtual memory)란 process 전체가 메모리에 올라오지 않더라도 실행이 가능하도록 하는 기법이다. 가상 메모리 기법을 통해 사용자 프로그램이 물리적 메모리보다 커져도 실행이 가능하다는 장점이 있다.

실제 우리가 사용하고 있는 운영체제에서는 가상메모리를 사용하고 있기 때문에 가상메모리를 이해하는 것은 개발자에게 중요하다.

### 가상메모리(virtual memory)

가상메모리는 실제의 물리 메모리 개념과 개발자 입장의 논리 메모리 개념을 분리한 것이다. 이렇게 함으로써 개발자가 메모리 크기에 관련한 문제를 염려할 필요 없이 쉽게 프로그램을 작성할 수 있게 해준다.

운영체제는 가상 메모리 기법을 통해 프로그램의 논리적주소 영역에서 필요한 부분만 물리적 메모리에 적재하고, 직접적으로 필요하지 않은 메모리 공간은 디스크(Swap 영역)에 저장하게 된다.

![4](https://github.com/EN-CS-STUDY/CS_STUDY/assets/81848426/441280e8-26d4-45aa-b77a-fb75fd15674a)

### 요구 페이징(demand paging)

**당장 사용**될 주소 공간을 **page 단위**로 메모리에 적재하는 방법을 **요구 페이징(demand paging)**이라고 한다. 요구 페이징 기법에서는 특정 page에 대해 cpu의 요청이 들어온 후에 해당 page를 메모리에 적재한다. 당장 실행에 필요한 page만을 메모리에 적재하기 때문에 메모리 사용량이 감소하고, 프로세스 전체를 메모리에 적재하는 입출력 오버헤드도 감소하는 장점이 있다.

요구 페이징 기법에서는 **유효/무효 비트(valid/invalid bit)**를 두어 각 page가 메모리에 존재하는지 표시하게 된다.

### Page fault

CPU가 **무효 비트(invalid bit)로 표시된 page에 엑세스**하는 상황을 **page fault**라고 한다. 위의 사진을 봤을 때 B를 읽고싶은데 물리 메모리에 없는 걸 볼 수 있다. 이걸 보고 page fault라고 한다.

CPU가 무효 page에 접근하면 주소 변환을 담당하는 하드웨어인 **MMU**가 **page fault trap을 발생**시키게 되고, 다음과 같은 순서로 page fault를 처리하게 된다.

cf)MMU(Memory Management Unit)란?

가상 주소를 물리 메모리 주소로 변환해주는 하드웨어 장치이다.

1. CPU가 페이지 N을 참조
2. Page table에서 페이지 N이 무효 상태임을 확인
3. MMU에서 Page fault trap이 발생
4. 디스크에서 페이지 N을 빈 프레임에 적재하고 page table을 업데이트(invalid→ valid)

![5](https://github.com/EN-CS-STUDY/CS_STUDY/assets/81848426/f4892540-ee34-477e-9bd3-e4ae0e2949f7)

page fault 처리 과정

### page 교체 알고리즘(replacement algorithm)

page fault가 발생하면, 요청된 page를 디스크에서 메모리로 가져온다. 이 때, 물리적 메모리에 공간이 부족할 수 있다. 그럴 경우에는 메모리에 올라와 있는 page를 디스크로 옮겨서 메모리 공간을 확보해야 한다.

이것을 페이지 교체(page replacement)라고 하고, 어떤 page를 교체할 것이냐를 결정하는 알고리즘이 page교체 알고리즘(replacement algorithm)이다.

교체 알고리즘은 최대한 page fault가 적게 일어나도록 도와줘야 한다. 따라서 앞으로 참조될 가능성이 적은 page를 선택해서 교체하는 것이 성능을 향상시키는 방법다.

| 알고리즘                   | 설명                                                                               |
| -------------------------- | ---------------------------------------------------------------------------------- |
| FIFO(First In First Out)   | 메모리에 올라온지 가장 오래된 page를 교체한다.                                     |
| 최적 페이지 교체           | 앞으로 가장 오랫동안 사용되지 않을 page를 찾아 교체한다. 실제구현은 어렵다         |
| LRU(Least Recently Used)   | 가장 오랫동안 사용되지 않은 page를 교체한다.                                       |
| LFU(Least Frequently Used) | 참조 횟수가 가장 적은 page를 교체한다. 비용대비 성능이 좋지 않아 잘 쓰이진 않는다. |

**요구페이징(demand paging)이란**

요구 페이징 기법은 특정 page에 대해 cpu의 요청이 들어 왔을 때 해당 page를 메모리에 적재합니다. 당장 실행에 필요한 page만을 메모리에 적재하기 때문에 메모리 사용량이 감소하고, 프로세스 전체를 메모리에 적재하는 입출력 오버헤드도 감소하는 장점이 있습니다

**페이지 교체 알고리즘(replacement algorithm)이란**

FIFO, 최적 페이지 교체, LRU, LFU 등이 있습니다.

**LRU알고리즘 vs LFU 알고리즘**

LRU는 Least Recently Used의 약자로, 가장 오래전에 참조가 이루어진 page를 교체한다.

LFU는 Least Frequently Used의 약자로, 물리적 메모리 내에 존재하는 page 중에서 지금까지의 참조횟수가 가장 적은 page를 교체한다.
