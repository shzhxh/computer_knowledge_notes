#### 简介

基本的特性和类型，代表了类型的基本属性。

按照Rust类型的固有属性，可以有多种分类方式。这些分类用特性来表示。

#### 宏

```
Copy	// 派生宏，生成Copy特性
```



#### 结构体

```
PhantomData	// 长度为0的类型，这种类型表现的像它们拥有<T>。
  pub struct PhantomData<T: ?Sized>;
PhantomPinned	// 一个marker类型，它不实现Unpin。
```



#### 特性

```
DiscriminantKind
StructuralEq
StructuralPartialEq
Unsize
Copy
Send
Sized
Sync
Unpin
```

