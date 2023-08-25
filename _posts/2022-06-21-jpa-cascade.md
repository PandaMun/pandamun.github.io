---
layout: post
title: JPA CASCADE(영속성 전이)
subtitle: CASCADE란 무엇인가? 그리고 CascadeType 종류에 대해서 알아보자
categories: JPA
---

최근 JPA를 공부했던 CascadeType, Cascade 범위에 대하여 정리해보았습니다.

### CASCADE 란

한 엔티티가 영속화(persist) 연관된 엔티티 또한 같이 영속화 되며, 삭제될때 또한 연관된 엔티티도 삭제되는등  특정 엔티티를 영속상태로 만들때 연관된 엔티티 또한 함께 영속 상태로 전이 되는것

![Define_CASCADE.png](/img/post/Define_CASCADE.png)

The cascade persist is used to specify that if an entity is persisted then all its associated child entities will also be persisted.

(영속성 전이는 특정한 작업을 수행한 엔티티가 영속화될때 관련된 모든 하위 엔티티도 영속화됨)

### CascadeType
JPA는 연관된 엔티티의 의존성을 설정할수 있도록 Enum 타입의 javax.persistence.CascadeType을 제공하고 있습니다.

![enum_CascadeType.png](/img/post/enum_CascadeType.png)

CascadeType은 PERSIST, MERGE,DETACH, PREFRESH, REMOVE, ALL 로 구성되어 있습니다.

- **CascadeType.PERSIST** : 엔티티를 영속화할 때, 연관된 엔티티도 함께 영속화합니다
- **CascadeType.MERGE** : 엔티티 상태를 병합(Merge)할 때, 연관된 엔티티도 모두 병합
- **CascadeType.DETACH** : 부모 엔티티를 detach() 수행하면, 연관 엔티티도 detach()상태가 되어 변경 사항 반영 되지 않습니다.
- **CascadeType.REFRESH** : 상위 엔티티를 새로고침(Refresh)할 때, 연관된 엔티티도 모두 새로고침
- **CascadeType.REMOVE** : 엔티티를 제거할 때, 연관된 엔티티도 모두 제거
- **CascadeType.ALL** : 위의 모든 Cascade 작업을 적용합니다.

### Cascade를 언제 어떻게 사용해야 하는가?

Cascade 되는 엔티티가 Cascade를 설정하는 엔티티에서만 참조되어야 합니다. 또한 두 엔티티의 라이프 사이클이 동일하거나 비슷해야합니다.

### Reference

- 김영한의 JPA
