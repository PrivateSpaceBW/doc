# Scene 类 API 文档

`Scene` 类是该库的核心类，包含了分析项目所需的所有内容。通过 Scene 类，可以重新生成项目的代码。

## 基本信息

`Scene` 类负责管理项目的文件、类、方法、命名空间等元素，并提供了丰富的 API 来访问和操作这些元素。

## 主要功能

### 场景构建

```typescript
buildSceneFromProjectDir(sceneConfig: SceneConfig): void
```

根据 `SceneConfig` 构建场景对象。该方法实现三个功能：

1. 从 `SceneConfig` 构建场景对象
2. 生成 `ArkFile` 文件
3. 收集项目导入信息

```typescript
buildSceneFromFiles(sceneConfig: SceneConfig): void
```

从文件构建场景，设置基本信息并按依赖顺序获取文件。

```typescript
buildScene4HarmonyProject(): void
```

为鸿蒙项目构建场景。首先解析项目路径，然后获取依赖关系，为项目构建 `ModuleScene`，最后构建所有方法体、生成扩展类并添加默认构造函数。

```typescript
buildSdk(sdkName: string, sdkPath: string): void
```

构建 SDK，解析 SDK 路径下的所有文件并生成 `ArkFile`。

### 基本信息获取

```typescript
getRealProjectDir(): string
```

获取当前项目的绝对路径。

```typescript
getProjectName(): string
```

返回项目的名称。

```typescript
getProjectFiles(): string[]
```

获取项目文件列表。

```typescript
getOptions(): SceneOptions
```

获取场景配置选项。

### 文件操作

```typescript
getFile(fileSignature: FileSignature): ArkFile | null
```

根据文件签名返回文件。如果找不到文件，则返回 `null`。

```typescript
getFiles(): ArkFile[]
```

获取场景中的所有文件。

```typescript
setFile(file: ArkFile): void
```

设置文件到场景中。

```typescript
removeFile(file: ArkFile): boolean
```

从场景中移除文件。

```typescript
getSdkArkFiles(): ArkFile[]
```

获取 SDK 的 ArkFile 文件列表。

```typescript
hasSdkFile(fileSignature: FileSignature): boolean
```

检查是否存在指定签名的 SDK 文件。

### 命名空间操作

```typescript
getNamespace(namespaceSignature: NamespaceSignature): ArkNamespace | null
```

根据命名空间签名获取命名空间。

```typescript
getNamespaces(): ArkNamespace[]
```

获取所有命名空间。

```typescript
getNamespacesMap(): Map<string, ArkNamespace>
```

获取命名空间映射表。

```typescript
removeNamespace(namespace: ArkNamespace): boolean
```

移除指定的命名空间。

### 类操作

```typescript
getClass(classSignature: ClassSignature): ArkClass | null
```

根据类签名获取类。

```typescript
getClasses(): ArkClass[]
```

获取所有类。

```typescript
getClassesMap(refresh?: boolean): Map<string, ArkClass>
```

获取类映射表，可选是否刷新。

```typescript
removeClass(arkClass: ArkClass): boolean
```

移除指定的类。

```typescript
getClassMap(): Map<FileSignature | NamespaceSignature, ArkClass[]>
```

获取文件或命名空间到类数组的映射。

### 方法操作

```typescript
getMethod(methodSignature: MethodSignature, refresh?: boolean): ArkMethod | null
```

根据方法签名获取方法。

```typescript
getMethods(): ArkMethod[]
```

获取所有方法。

```typescript
getMethodsMap(refresh?: boolean): Map<string, ArkMethod>
```

获取方法映射表，可选是否刷新。

```typescript
addToMethodsMap(method: ArkMethod): void
```

将方法添加到方法映射表中。

```typescript
removeMethod(method: ArkMethod): boolean
```

移除指定的方法。

```typescript
getStaticInitMethods(): ArkMethod[]
```

获取所有静态初始化方法。

### 模块操作

```typescript
getModuleScene(moduleName: string): ModuleScene | undefined
```

根据模块名获取模块场景。

```typescript
getModuleSceneMap(): Map<string, ModuleScene>
```

获取模块场景映射表。

```typescript
buildModuleScene(moduleName: string, modulePath: string, supportFileExts: string[]): void
```

构建模块场景。

### 依赖分析

```typescript
getOhPkgContent(): any
```

获取 oh-package.json5 内容。

```typescript
getOhPkgContentMap(): Map<string, any>
```

获取 oh-package.json5 内容映射表。

```typescript
getOhPkgFilePath(): string
```

获取 oh-package.json5 文件路径。

```typescript
getOverRides(): Map<string, any>
```

获取覆盖配置。

```typescript
getOverRideDependencyMap(): Map<string, any>
```

获取覆盖依赖映射表。

### 调用图构建

```typescript
makeCallGraphCHA(entryPoints: ArkMethod[]): CallGraph
```

使用类层次分析（CHA）构建调用图。

```typescript
makeCallGraphRTA(entryPoints: ArkMethod[]): CallGraph
```

使用快速类型分析（RTA）构建调用图。

### 类型推断

```typescript
inferTypes(): void
```

为每个非默认方法推断类型，包括字段、局部变量和引用的类型。

```typescript
inferSimpleTypes(): void
```

推断简单类型，遍历所有方法中的赋值语句，根据右操作数的类型设置左操作数的类型。

### 全局变量

```typescript
getVisibleValue(): VisibleValue
```

获取当前作用域中可见的值。

```typescript
getGlobalVariableMap(): Map<FileSignature | NamespaceSignature, Local[]>
```

获取全局变量映射表。

### 其他工具方法

```typescript
getSdkGlobal(globalName: string): any | null
```

获取 SDK 全局变量。

```typescript
getUnhandledFilePaths(): string[]
```

获取当前无法处理的文件路径。

```typescript
getUnhandledSdkFilePaths(): string[]
```

获取当前无法处理的 SDK 文件路径。

```typescript
getModuleSdkMap(): Map<string, any[]>
```

获取模块 SDK 映射表。

```typescript
getProjectSdkMap(): Map<string, any>
```

获取项目 SDK 映射表。

```typescript
getGlobalModule2PathMapping(): any
```

获取全局模块到路径的映射。

```typescript
getbaseUrl(): string
```

获取基础 URL。

```typescript
buildClassDone(): boolean
```

检查类构建是否完成。

## ModuleScene 类

`ModuleScene` 类表示项目中的一个模块场景。

### 主要方法

```typescript
ModuleSceneBuilder(moduleName: string, modulePath: string, supportFileExts: string[], recursively?: boolean): void
```

构建模块场景。

```typescript
ModuleScenePartiallyBuilder(moduleName: string, modulePath: string): void
```

部分构建模块场景。

```typescript
getModuleName(): string
```

获取模块名称。

```typescript
getModulePath(): string
```

获取模块路径。

```typescript
getOhPkgFilePath(): string
```

获取模块的 oh-package.json5 文件路径。

```typescript
getOhPkgContent(): any
```

获取模块的 oh-package.json5 内容。

```typescript
getModuleFilesMap(): Map<string, ArkFile>
```

获取模块文件映射表。

```typescript
addArkFile(arkFile: ArkFile): void
```

添加 ArkFile 到模块场景。

## 使用示例

### 构建场景并推断类型

```typescript
// 构建配置
const projectDir = "path/to/project";
const sceneConfig = new SceneConfig();
sceneConfig.buildFromProjectDir(projectDir);

// 构建场景
const scene = new Scene();
scene.buildSceneFromProjectDir(sceneConfig);
scene.inferTypes();
```

### 获取项目中的所有类和方法

```typescript
const scene = new Scene();
scene.buildSceneFromProjectDir(sceneConfig);

// 获取所有类
const classes = scene.getClasses();

// 获取所有方法
const methods = scene.getMethods();
```

### 构建调用图

```typescript
const scene = new Scene();
scene.buildSceneFromProjectDir(sceneConfig);

// 获取入口点方法
const entryPoints = scene.getEntryPoints();

// 使用类层次分析构建调用图
const callGraph = scene.makeCallGraphCHA(entryPoints);
```
