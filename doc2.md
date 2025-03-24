# Scene 类 API 文档

`Scene` 类是该库的核心类，包含了分析项目所需的所有内容。通过 Scene 类，可以重新生成项目的代码。

## 基本信息

`Scene` 类负责管理项目的文件、类、方法、命名空间等元素，并提供了丰富的 API 来访问和操作这些元素。

## 主要功能

### 场景构建

#### buildSceneFromProjectDir(sceneConfig: SceneConfig): void

根据 `SceneConfig` 构建场景对象。该方法实现三个功能：

1. 从 `SceneConfig` 构建场景对象
2. 生成 `ArkFile` 文件
3. 收集项目导入信息

```typescript
// 构建配置
const projectDir = "path/to/project";
const sceneConfig = new SceneConfig();
sceneConfig.buildFromProjectDir(projectDir);

// 构建场景
const scene = new Scene();
scene.buildSceneFromProjectDir(sceneConfig);
```

#### buildSceneFromFiles(sceneConfig: SceneConfig): void

从文件构建场景，设置基本信息并按依赖顺序获取文件。

#### buildScene4HarmonyProject(): void

为鸿蒙项目构建场景。首先解析项目路径，然后获取依赖关系，为项目构建 `ModuleScene`，最后构建所有方法体、生成扩展类并添加默认构造函数。

```typescript
// 从配置文件构建场景
let config: SceneConfig = new SceneConfig();
config.buildFromJson(join(__dirname, '../resources/CountDown/arkanalyzer_config.json'));
let scene: Scene = new Scene();
scene.buildBasicInfo(config);
scene.buildScene4HarmonyProject();
scene.collectProjectImportInfos();
```

#### buildSdk(sdkName: string, sdkPath: string): void

构建 SDK，解析 SDK 路径下的所有文件并生成 `ArkFile`。

#### buildBasicInfo(config: SceneConfig): void

设置场景的基本信息，包括项目路径、名称等。

```typescript
let config: SceneConfig = new SceneConfig();
config.buildFromJson(join(__dirname, '../resources/CountDown/arkanalyzer_config.json'));
let scene: Scene = new Scene();
scene.buildBasicInfo(config);
```

### 基本信息获取

#### getRealProjectDir(): string

获取当前项目的绝对路径。

#### getProjectName(): string

返回项目的名称。

#### getProjectFiles(): string[]

获取项目文件列表。

#### getOptions(): SceneOptions

获取场景配置选项。

### 文件操作

#### getFile(fileSignature: FileSignature): ArkFile | null

根据文件签名返回文件。如果找不到文件，则返回 `null`。

#### getFiles(): ArkFile[]

获取场景中的所有文件。

```typescript
// 获取所有文件
let files: ArkFile[] = scene.getFiles();
let fileNames: string[] = files.map((file) => file.getName());
console.log(fileNames);
```

#### setFile(file: ArkFile): void

设置文件到场景中。

#### removeFile(file: ArkFile): boolean

从场景中移除文件。

#### getSdkArkFiles(): ArkFile[]

获取 SDK 的 ArkFile 文件列表。

#### hasSdkFile(fileSignature: FileSignature): boolean

检查是否存在指定签名的 SDK 文件。

### 命名空间操作

#### getNamespace(namespaceSignature: NamespaceSignature): ArkNamespace | null

根据命名空间签名获取命名空间。

#### getNamespaces(): ArkNamespace[]

获取所有命名空间。

```typescript
// 获取命名空间
let namespaces: ArkNamespace[] = scene.getNamespaces();
let namespaceNames: string[] = namespaces.map((ns) => ns.getName());
console.log(namespaceNames);
```

#### getNamespacesMap(): Map<string, ArkNamespace>

获取命名空间映射表。

#### removeNamespace(namespace: ArkNamespace): boolean

移除指定的命名空间。

### 类操作

#### getClass(classSignature: ClassSignature): ArkClass | null

根据类签名获取类。

#### getClasses(): ArkClass[]

获取所有类。

```typescript
// 获取所有类
let classes: ArkClass[] = scene.getClasses();
let classNames: string[] = classes.map((cls) => cls.getName());
console.log(classNames);

// 获取特定类
let bookClass: ArkClass = classes.filter((value) => value.getName() == 'Book')[0];
```

#### getClassesMap(refresh?: boolean): Map<string, ArkClass>

获取类映射表，可选是否刷新。

#### removeClass(arkClass: ArkClass): boolean

移除指定的类。

#### getClassMap(): Map<FileSignature | NamespaceSignature, ArkClass[]>

获取文件或命名空间到类数组的映射。

### 方法操作

#### getMethod(methodSignature: MethodSignature, refresh?: boolean): ArkMethod | null

根据方法签名获取方法。

#### getMethods(): ArkMethod[]

获取所有方法。

```typescript
// 获取所有方法
let methods: ArkMethod[] = scene.getMethods();
let methodNames: string[] = methods.map((mthd) => mthd.getName());
console.log(methodNames);

// 获取特定类的方法
let serviceClass: ArkClass = classes.filter((value) => value.getName() === 'BookService')[0];
let methods: ArkMethod[] = serviceClass.getMethods();
```

#### getMethodsMap(refresh?: boolean): Map<string, ArkMethod>

获取方法映射表，可选是否刷新。

#### addToMethodsMap(method: ArkMethod): void

将方法添加到方法映射表中。

#### removeMethod(method: ArkMethod): boolean

移除指定的方法。

#### getStaticInitMethods(): ArkMethod[]

获取所有静态初始化方法。

### 模块操作

#### getModuleScene(moduleName: string): ModuleScene | undefined

根据模块名获取模块场景。

#### getModuleSceneMap(): Map<string, ModuleScene>

获取模块场景映射表。

#### buildModuleScene(moduleName: string, modulePath: string, supportFileExts: string[]): void

构建模块场景。

### 依赖分析

#### getOhPkgContent(): any

获取 oh-package.json5 内容。

#### getOhPkgContentMap(): Map<string, any>

获取 oh-package.json5 内容映射表。

#### getOhPkgFilePath(): string

获取 oh-package.json5 文件路径。

#### getOverRides(): Map<string, any>

获取覆盖配置。

#### getOverRideDependencyMap(): Map<string, any>

获取覆盖依赖映射表。

### 调用图构建

#### makeCallGraphCHA(entryPoints: ArkMethod[]): CallGraph

使用类层次分析（CHA）构建调用图。

```typescript
// 指定入口点函数
let entryPoints: MethodSignature[] = [];
for (const method of scene.getMethods()) {
    if (method.getName() === 'main' && method.getDeclaringArkFile().getName().endsWith('main.ts')) {
        entryPoints.push(method.getSignature());
    }
}

// 构建方法调用图
let callGraph = scene.makeCallGraphCHA(entryPoints);
callGraph.dump('./out/CHA.dot');
```

#### makeCallGraphRTA(entryPoints: ArkMethod[]): CallGraph

使用快速类型分析（RTA）构建调用图。

```typescript
// 构建方法调用图
let callGraph = scene.makeCallGraphRTA(entryPoints);
callGraph.dump('./out/RTA.dot');
```

### 类型推断

#### inferTypes(): void

为每个非默认方法推断类型，包括字段、局部变量和引用的类型。

```typescript
// 类型推导
scene.inferTypes();
```

#### inferSimpleTypes(): void

推断简单类型，遍历所有方法中的赋值语句，根据右操作数的类型设置左操作数的类型。

### 控制流图分析

#### 获取方法的控制流图(CFG)

可以通过方法获取其控制流图，并进行可视化或分析。

```typescript
// 获取方法getBooksByAuthor的CFG
let method = serviceClass.getMethodWithName('getBooksByAuthor');
let cfg = method?.getBody()?.getCfg();
let dotMethodPrinter = new DotMethodPrinter(method!);
PrinterBuilder.dump(dotMethodPrinter, 'out/getBooksByAuthor_cfg.dot');
```

#### 定义-使用链分析

可以分析方法中变量的定义-使用链。

```typescript
const cfg = method.getBody()!.getCfg();
cfg.buildDefUseChain();
for (const chain of cfg.getDefUseChains()) {
    console.log(
        `variable: ${chain.value.toString()}, def: ${chain.def.toString()}, use: ${chain.use.toString()}`
    );
}
```

#### 静态单赋值形式(SSA)转换

可以将方法体转换为静态单赋值形式。

```typescript
let ssaFormer = new StaticSingleAssignmentFormer();
ssaFormer.transformBody(body);
```

### 视图树分析

#### 获取UI组件的视图树

对于鸿蒙项目，可以获取UI组件的视图树并进行分析。

```typescript
// 读取组件ViewTree
let arkFile = scene.getFiles().find((file) => file.getName().endsWith('CountDown.ets'));
let arkClass = arkFile?.getClassWithName('CountDown');
let vt = arkClass?.getViewTree();
// 从根节点遍历UI组件
let root = vt?.getRoot();
root?.walk((item) => {
    // 处理视图树节点
    return false;
});
PrinterBuilder.dump(new ViewTreePrinter(vt!), 'out/CountDownViewTree.dot');
```

### 全局变量

#### getVisibleValue(): VisibleValue

获取当前作用域中可见的值。

#### getGlobalVariableMap(): Map<FileSignature | NamespaceSignature, Local[]>

获取全局变量映射表。

### 其他工具方法

#### getSdkGlobal(globalName: string): any | null

获取 SDK 全局变量。

#### getUnhandledFilePaths(): string[]

获取当前无法处理的文件路径。

#### getUnhandledSdkFilePaths(): string[]

获取当前无法处理的 SDK 文件路径。

#### getModuleSdkMap(): Map<string, any[]>

获取模块 SDK 映射表。

#### getProjectSdkMap(): Map<string, any>

获取项目 SDK 映射表。

#### getGlobalModule2PathMapping(): any

获取全局模块到路径的映射。

#### getbaseUrl(): string

获取基础 URL。

#### buildClassDone(): boolean

检查类构建是否完成。

### 代码分析工具

#### 未定义变量检查

可以检查代码中的未定义变量问题。

```typescript
// 创建Checker
const problem = new UndefinedVariableChecker(
    [...method.getCfg()!.getBlocks()][0].getStmts()[method.getParameters().length],
    method
);

// Solver求解
const solver = new UndefinedVariableSolver(problem, scene);
solver.solve();

// 输出错误报告
for (const outcome of problem.getOutcomes()) {
    let position = outcome.stmt.getOriginPositionInfo();
    console.log('undefined error in line ' + position.getLineNo());
}
```

## ModuleScene 类

`ModuleScene` 类表示项目中的一个模块场景。

### 主要方法

#### ModuleSceneBuilder(moduleName: string, modulePath: string, supportFileExts: string[], recursively?: boolean): void

构建模块场景。

#### ModuleScenePartiallyBuilder(moduleName: string, modulePath: string): void

部分构建模块场景。

#### getModuleName(): string

获取模块名称。

#### getModulePath(): string

获取模块路径。

#### getOhPkgFilePath(): string

获取模块的 oh-package.json5 文件路径。

#### getOhPkgContent(): any

获取模块的 oh-package.json5 内容。

#### getModuleFilesMap(): Map<string, ArkFile>

获取模块文件映射表。

#### addArkFile(arkFile: ArkFile): void

添加 ArkFile 到模块场景。
