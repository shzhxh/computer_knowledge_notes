#### 简介

> marker的意思是标记物，我想因为这个模块下所包含的特性非常普遍，以至于可以用来标记其它的对象，所以用marker这个词。

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
Send	# 如果一个值的类型实现了Send，那么这个值的所有权就可以在线程间传递。
Sized
Sync	# 多个线程可安全地引用实现了Sync的类型。
Unpin
```

