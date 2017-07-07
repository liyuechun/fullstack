### 类

ES6类是基于原型的面相对象模式的简单糖。 拥有一个方便的声明式表单使得类模式更易于使用，并易于操作。 类支持基于原型的继承，父类调用，实例、静态方法和构造函数。


```JavaScript
class SkinnedMesh extends THREE.Mesh {
  constructor(geometry, materials) {
    super(geometry, materials);

    this.idMatrix = SkinnedMesh.defaultMatrix();
    this.bones = [];
    this.boneMatrices = [];
    //...
  }
  update(camera) {
    //...
    super.update();
  }
  get boneCount() {
    return this.bones.length;
  }
  set matrixType(matrixType) {
    this.idMatrix = SkinnedMesh[matrixType]();
  }
  static defaultMatrix() {
    return new THREE.Matrix4();
  }
}
```

更多信息: [MDN Classes](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)

