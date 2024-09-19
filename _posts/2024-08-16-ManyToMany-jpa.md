---
layout: post
title: @ManyToMany를 사용하지 않는 이유
subtitle: 다대다 관계는 일대다&다대일로 풀어내자
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/path.jpg
tags: [spring-boot, jpa]
author: EomYoosang
---

# JPA에서 @ManyToMany를 사용하지 않는 이유

## @ManyToMany
`@ManyToMany` 어노테이션은 다대다 관계를 가지는 두 객체에 사용합니다.
사용 예시는 아래와 같습니다.

```java
@Entity
public class A {

    @Id @GeneratedValue
    @Column(name = "a_id")
    private Long id;

    @ManyToMany
    @JoinTable(name = "a_b")
    private List<b> bList = new ArrayList<>();
}
.
.
.
@Entity
public class B {

    @Id @GeneratedValue
    @Column(name = "b_id")
    private Long id;

    @ManyToMany(mappedBy = "bList")
    private List<a> aList = new ArrayList<>();
}
```

위의 클래스 A, B는 `@ManyToMany`를 사용하여 연관관계매핑되어 있습니다.
연관관계의 주인은 'A'이고 다대다 관계를 관계형 DB에서 구현할 수 없으므로 하이버네이트에서 `a_b`라는 테이블을 자동으로 생성하여 사용합니다.

## @ManyToMany를 사용하면 안되는 이유는?

위의 예시에서 클래스 A, B의 연관관계를 매핑하기 위해 중간 테이블을 자동으로 생성해주는 것은 편리해보입니다. 하지만 `@ManyToMany`로 생성된 테이블은 비즈니스 로직상 필요한 컬럼을 추가할 수 없다는 치명적인 단점이 있습니다. 

따라서 실무에서는 절대로 사용해서는 안됩니다.

## @ManyToMany는 @OneToMany, @ManyToOne으로 풀어내자

실무에서 다대다 관계를 매핑하기 위해서는 두 객체 사이에 중간 객체를 만들어주어야 합니다.

```java
@Entity
public class A {

    @Id @GeneratedValue
    @Column(name = "a_id")
    private Long id;

    @OneToMany(mappedBy = "A")
    private List<AB> ABList = new ArrayList<>();
}
.
.
.
@Entity
public class B {

    @Id @GeneratedValue
    @Column(name = "b_id")
    private Long id;

    @OneToMany(mappedBy = "B")
    private List<AB> ABList = new ArrayList<>();
}
.
.
.
@Entity
public class AB {

    @Id @GeneratedValue
    @Column(name = "ab_id")
    private Long id;

    @ManyToOne
    @JoinColumn(name = "a_id")
    private A a;

    @ManyToOne
    @JoinColumn(name = "b_id")
    private B b;
}
```

위 코드의 경우 수동으로 생성해준 클래스 AB(테이블 ab)에 비즈니스 로직에 필요한 만큼 생성 일자 등 컬럼을 추가하여 수정할 수 있습니다.