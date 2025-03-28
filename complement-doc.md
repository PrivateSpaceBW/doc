`getMethods(): ArkMethod[]`



And what's the `ArkMethod` class?


# `ArkMethod` class
the complete code is as follows:

```ts
export declare class ArkMethod extends ArkBaseModel implements ArkExport {
    private code?;
    private declaringArkClass;
    private outerMethod?;
    private genericTypes?;
    private methodDeclareSignatures?;
    private methodDeclareLineCols?;
    private methodSignature?;
    private lineCol?;
    private body?;
    private viewTree?;
    private bodyBuilder?;
    private isGeneratedFlag;
    private asteriskToken;
    constructor();
    getExportType(): ExportType;
    getName(): string;
    /**
     * Returns the codes of method as a **string.**
     * @returns the codes of method.
     */
    getCode(): string | undefined;
    setCode(code: string): void;
    /**
     * Get all lines of the method's declarations or null if the method has no seperated declaration.
     * @returns null or the lines of the method's declarations with number type.
     */
    getDeclareLines(): number[] | null;
    /**
     * Get all columns of the method's declarations or null if the method has no seperated declaration.
     * @returns null or the columns of the method's declarations with number type.
     */
    getDeclareColumns(): number[] | null;
    /**
     * Set lines and columns of the declarations with number type inputs and then encoded them to LineCol type.
     * The length of lines and columns should be the same otherwise they cannot be encoded together.
     * @param lines - the number of lines.
     * @param columns - the number of columns.
     * @returns
     */
    setDeclareLinesAndCols(lines: number[], columns: number[]): void;
    /**
     * Set lineCols of the declarations directly with LineCol type input.
     * @param lineCols - the encoded lines and columns with LineCol type.
     * @returns
     */
    setDeclareLineCols(lineCols: LineCol[]): void;
    /**
     * Get encoded lines and columns of the method's declarations or null if the method has no seperated declaration.
     * @returns null or the encoded lines and columns of the method's declarations with LineCol type.
     */
    getDeclareLineCols(): LineCol[] | null;
    /**
     * Get line of the method's implementation or null if the method has no implementation.
     * @returns null or the number of the line.
     */
    getLine(): number | null;
    /**
     * Set line of the implementation with line number input.
     * The line number will be encoded together with the original column number.
     * @param line - the line number of the method implementation.
     * @returns
     */
    setLine(line: number): void;
    /**
     * Get column of the method's implementation or null if the method has no implementation.
     * @returns null or the number of the column.
     */
    getColumn(): number | null;
    /**
     * Set column of the implementation with column number input.
     * The column number will be encoded together with the original line number.
     * @param column - the column number of the method implementation.
     * @returns
     */
    setColumn(column: number): void;
    /**
     * Get encoded line and column of the method's implementation or null if the method has no implementation.
     * @returns null or the encoded line and column of the method's implementation with LineCol type.
     */
    getLineCol(): LineCol | null;
    /**
     * Set lineCol of the implementation directly with LineCol type input.
     * @param lineCol - the encoded line and column with LineCol type.
     * @returns
     */
    setLineCol(lineCol: LineCol): void;
    /**
     * Returns the declaring class of the method.
     * @returns The declaring class of the method.
     */
    getDeclaringArkClass(): ArkClass;
    setDeclaringArkClass(declaringArkClass: ArkClass): void;
    getDeclaringArkFile(): import("./ArkFile").ArkFile;
    isDefaultArkMethod(): boolean;
    isAnonymousMethod(): boolean;
    getParameters(): import("./builder/ArkMethodBuilder").MethodParameter[];
    getReturnType(): Type;
    /**
     * Get all declare signatures.
     * The results could be null if there is no seperated declaration of the method.
     * @returns null or the method declare signatures.
     */
    getDeclareSignatures(): MethodSignature[] | null;
    /**
     * Get the index of the matched method declare signature among all declare signatures.
     * The index will be -1 if there is no matched signature found.
     * @param targetSignature - the target declare signature want to search.
     * @returns -1 or the index of the matched signature.
     */
    getDeclareSignatureIndex(targetSignature: MethodSignature): number;
    /**
     * Get the method signature of the implementation.
     * The signature could be null if the method is only a declaration which body is undefined.
     * @returns null or the method implementation signature.
     */
    getImplementationSignature(): MethodSignature | null;
    /**
     * Get the method signature of the implementation or the first declaration if there is no implementation.
     * For a method, the implementation and declaration signatures must not be undefined at the same time.
     * A {@link MethodSignature} includes:
     * - Class Signature: indicates which class this method belong to.
     * - Method SubSignature: indicates the detail info of this method such as method name, parameters, returnType, etc.
     * @returns The method signature.
     * @example
     * 1. Get the signature of method mtd.

     ```typescript
     let signature = mtd.getSignature();
     // ... ...
```
     */
    getSignature(): MethodSignature;
    /**
     * Set signatures of all declarations.
     * It will reset the declaration signatures if they are already defined before.
     * @param signatures - one signature or a list of signatures.
     * @returns
     */
    setDeclareSignatures(signatures: MethodSignature | MethodSignature[]): void;
    /**
     * Reset signature of one declaration with the specified index.
     * Will do nothing if the index doesn't exist.
     * @param signature - new signature want to set.
     * @param index - index of signature want to set.
     * @returns
     */
    setDeclareSignatureWithIndex(signature: MethodSignature, index: number): void;
    /**
     * Set signature of implementation.
     * It will reset the implementation signature if it is already defined before.
     * @param signature - signature of implementation.
     * @returns
     */
    setImplementationSignature(signature: MethodSignature): void;
    getSubSignature(): import("./ArkSignature").MethodSubSignature;
    getGenericTypes(): GenericType[] | undefined;
    isGenericsMethod(): boolean;
    setGenericTypes(genericTypes: GenericType[]): void;
    getBodyBuilder(): BodyBuilder | undefined;
    /**
     * Get {@link ArkBody} of a Method.
     * A {@link ArkBody} contains the CFG and actual instructions or operations to be executed for a method.
     * It is analogous to the body of a function or method in high-level programming languages,
     * which contains the statements and expressions that define what the function does.
     * @returns The {@link ArkBody} of a method.
     * @example
     * 1. Get cfg or stmt through ArkBody.
    
     ```typescript
     let cfg = this.scene.getMethod()?.getBody().getCfg();
     const body = arkMethod.getBody()
     ```
    
     2. Get local variable through ArkBody.
    
     ```typescript
     arkClass.getDefaultArkMethod()?.getBody().getLocals.forEach(local=>{...})
     let locals = arkFile().getDefaultClass().getDefaultArkMethod()?.getBody()?.getLocals();
     ```
     */
    getBody(): ArkBody | undefined;
    setBody(body: ArkBody): void;
    /**
     * Get the CFG (i.e., control flow graph) of a method.
     * The CFG is a graphical representation of all possible control flow paths within a method's body.
     * A CFG consists of blocks, statements and goto control jumps.
     * @returns The CFG (i.e., control flow graph) of a method.
     * @example
     * 1. get stmt through ArkBody cfg.
    
     ```typescript
     body = arkMethod.getBody();
     const cfg = body.getCfg();
     for (const threeAddressStmt of cfg.getStmts()) {
     ... ...
     }
     ```
    
     2. get blocks through ArkBody cfg.
    
     ```typescript
     const body = arkMethod.getBody();
     const blocks = [...body.getCfg().getBlocks()];
     for (let i=0; i<blocks.length; i++) {
     const block = blocks[i];
     ... ...
     for (const stmt of block.getStmts()) {
     ... ...
     }
     let text = "next;"
     for (const next of block.getSuccessors()) {
     text += blocks.indexOf(next) + ' ';
     }
     // ... ...
     }
     ```
     */
    getCfg(): Cfg | undefined;
    getOriginalCfg(): Cfg | undefined;
    getParameterRefs(): ArkParameterRef[] | null;
    getParameterInstances(): Value[];
    getThisInstance(): Value | null;
    getReturnValues(): Value[];
    getReturnStmt(): Stmt[];
    setViewTree(viewTree: ViewTree): void;
    getViewTree(): ViewTree | undefined;
    hasViewTree(): boolean;
    setBodyBuilder(bodyBuilder: BodyBuilder): void;
    freeBodyBuilder(): void;
    buildBody(): void;
    isGenerated(): boolean;
    setIsGeneratedFlag(isGeneratedFlag: boolean): void;
    getAsteriskToken(): boolean;
    setAsteriskToken(asteriskToken: boolean): void;
    validate(): ArkError;
    matchMethodSignature(args: Value[]): MethodSignature;
    private matchParam;
    getOuterMethod(): ArkMethod | undefined;
    setOuterMethod(method: ArkMethod): void;
    getFunctionLocal(name: string): Local | null;
}
```

# `makeCallGraphCHA` method

`makeCallGraphCHA(entryPoints: MethodSignature[]): CallGraph;`

## `CallGraph` class

```
export declare class CallGraph extends BaseExplicitGraph {
    private scene;
    private idToCallSiteMap;
    private callSiteToIdMap;
    private stmtToCallSitemap;
    private stmtToDynCallSitemap;
    private methodToCGNodeMap;
    private callPairToEdgeMap;
    private callSiteNum;
    private entries;
    private cgStat;
    private dummyMainMethodID;
    constructor(s: Scene);
    private getCallPairString;
    getCallEdgeByPair(srcID: NodeID, dstID: NodeID): CallGraphEdge | undefined;
    addCallGraphNode(method: Method, kind?: CallGraphNodeKind): CallGraphNode;
    removeCallGraphNode(nodeID: NodeID): void;
    getCallGraphNodeByMethod(method: Method): CallGraphNode;
    addDirectOrSpecialCallEdge(caller: Method, callee: Method, callStmt: Stmt, isDirectCall?: boolean): void;
    removeCallGraphEdge(nodeID: NodeID): void;
    addDynamicCallInfo(callStmt: Stmt, caller: Method, protentialCallee?: Method): void;
    addDynamicCallEdge(callerID: NodeID, calleeID: NodeID, callStmt: Stmt): void;
    getDynCallsiteByStmt(stmt: Stmt): DynCallSite | undefined;
    addStmtToCallSiteMap(stmt: Stmt, cs: CallSite): boolean;
    getCallSiteByStmt(stmt: Stmt): CallSite | undefined;
    getDynEdges(): Map<Method, Set<Method>>;
    getMethodByFuncID(id: FuncID): Method | null;
    getArkMethodByFuncID(id: FuncID): ArkMethod | null;
    getEntries(): FuncID[];
    setEntries(n: NodeID[]): void;
    dump(name: string, entry?: FuncID): void;
    detectReachable(fromID: FuncID, dstID: FuncID): boolean;
    printStat(): void;
    getStat(): string;
    setDummyMainFuncID(dummyMainMethodID: number): void;
    getDummyMainFuncID(): FuncID | undefined;
    isUnknownMethod(funcID: FuncID): boolean;
    getGraphName(): string;
}
```

## `MethodSignature` class
```
export declare class MethodSignature {
    private declaringClassSignature;
    private methodSubSignature;
    constructor(declaringClassSignature: ClassSignature, methodSubSignature: MethodSubSignature);
    /**
     * Return the declaring class signature.
     * A {@link ClassSignature} includes:
     * - File Signature: including the **string** names of the project and file, respectively. The default value of project's name is "%unk" and the default value of file's name is "%unk".
     * - Namespace Signature | **null**:  it may be a namespace signature or **null**. A namespace signature can indicate its **string** name of namespace and its file signature.
     * - Class Name: the **string** name of this class.
     * @returns The declaring class signature.
     * @example
     * 1. get class signature from ArkMethod.

     ```typescript
     let methodSignature = expr.getMethodSignature();
     let name = methodSignature.getDeclaringClassSignature().getClassName();
     ```
     *
     */
    getDeclaringClassSignature(): ClassSignature;
    /**
     * Returns the sub-signature of this method signature.
     * The sub-signature is part of the method signature, which is used to
     * identify the name of the method, its parameters and the return value type.
     * @returns The sub-signature of this method signature.
     */
    getMethodSubSignature(): MethodSubSignature;
    getType(): Type;
    toString(): string;
    toMapKey(): string;
    isMatch(signature: MethodSignature): boolean;
    getParamLength(): number;
}
```

# call graph algorithms

only have two following algorithms

```ts
makeCallGraphCHA(entryPoints: MethodSignature[]): CallGraph;
makeCallGraphRTA(entryPoints: MethodSignature[]): CallGraph;
```

# how to extract method signatures

I think the above `MethodSignature` class will help you about it.

# how to access the call graph results

## `CallGraph` class

```
export declare class CallGraph extends BaseExplicitGraph {
    private scene;
    private idToCallSiteMap;
    private callSiteToIdMap;
    private stmtToCallSitemap;
    private stmtToDynCallSitemap;
    private methodToCGNodeMap;
    private callPairToEdgeMap;
    private callSiteNum;
    private entries;
    private cgStat;
    private dummyMainMethodID;
    constructor(s: Scene);
    private getCallPairString;
    getCallEdgeByPair(srcID: NodeID, dstID: NodeID): CallGraphEdge | undefined;
    addCallGraphNode(method: Method, kind?: CallGraphNodeKind): CallGraphNode;
    removeCallGraphNode(nodeID: NodeID): void;
    getCallGraphNodeByMethod(method: Method): CallGraphNode;
    addDirectOrSpecialCallEdge(caller: Method, callee: Method, callStmt: Stmt, isDirectCall?: boolean): void;
    removeCallGraphEdge(nodeID: NodeID): void;
    addDynamicCallInfo(callStmt: Stmt, caller: Method, protentialCallee?: Method): void;
    addDynamicCallEdge(callerID: NodeID, calleeID: NodeID, callStmt: Stmt): void;
    getDynCallsiteByStmt(stmt: Stmt): DynCallSite | undefined;
    addStmtToCallSiteMap(stmt: Stmt, cs: CallSite): boolean;
    getCallSiteByStmt(stmt: Stmt): CallSite | undefined;
    getDynEdges(): Map<Method, Set<Method>>;
    getMethodByFuncID(id: FuncID): Method | null;
    getArkMethodByFuncID(id: FuncID): ArkMethod | null;
    getEntries(): FuncID[];
    setEntries(n: NodeID[]): void;
    dump(name: string, entry?: FuncID): void;
    detectReachable(fromID: FuncID, dstID: FuncID): boolean;
    printStat(): void;
    getStat(): string;
    setDummyMainFuncID(dummyMainMethodID: number): void;
    getDummyMainFuncID(): FuncID | undefined;
    isUnknownMethod(funcID: FuncID): boolean;
    getGraphName(): string;
}
```

# `getMethodByFuncID` method

`getMethodByFuncID(id: FuncID): Method | null;`

and I find:

`export type Method = MethodSignature;`

about the `MethodSignature` class:

## `MethodSignature` class

```
export declare class MethodSignature {
    private declaringClassSignature;
    private methodSubSignature;
    constructor(declaringClassSignature: ClassSignature, methodSubSignature: MethodSubSignature);
    /**
     * Return the declaring class signature.
     * A {@link ClassSignature} includes:
     * - File Signature: including the **string** names of the project and file, respectively. The default value of project's name is "%unk" and the default value of file's name is "%unk".
     * - Namespace Signature | **null**:  it may be a namespace signature or **null**. A namespace signature can indicate its **string** name of namespace and its file signature.
     * - Class Name: the **string** name of this class.
     * @returns The declaring class signature.
     * @example
     * 1. get class signature from ArkMethod.

     ```typescript
     let methodSignature = expr.getMethodSignature();
     let name = methodSignature.getDeclaringClassSignature().getClassName();
```
     *
     */
    getDeclaringClassSignature(): ClassSignature;
    /**
     * Returns the sub-signature of this method signature.
     * The sub-signature is part of the method signature, which is used to
     * identify the name of the method, its parameters and the return value type.
     * @returns The sub-signature of this method signature.
     */
    getMethodSubSignature(): MethodSubSignature;
    getType(): Type;
    toString(): string;
    toMapKey(): string;
    isMatch(signature: MethodSignature): boolean;
    getParamLength(): number;
}
```

# `getDynEdges()` method

`getDynEdges(): Map<Method, Set<Method>>;`

and an information about the `Method`: `export type Method = MethodSignature;`

# what's the type of  `callee`

I find it in the souce code:

`addDirectOrSpecialCallEdge(caller: Method, callee: Method, callStmt: Stmt, isDirectCall?: boolean): void;`

so we can infer that `callee`'s type is `Method`. And we know that `Method` just is an alias of `MethodSignature`.

# Method Name Extraction

```
/**
 * @category core/model
 */
 export declare class MethodSignature {
    private declaringClassSignature;
    private methodSubSignature;
    constructor(declaringClassSignature: ClassSignature, methodSubSignature: MethodSubSignature);
    /**
     * Return the declaring class signature.
     * A {@link ClassSignature} includes:
     * - File Signature: including the **string** names of the project and file, respectively. The default value of project's name is "%unk" and the default value of file's name is "%unk".
     * - Namespace Signature | **null**:  it may be a namespace signature or **null**. A namespace signature can indicate its **string** name of namespace and its file signature.
     * - Class Name: the **string** name of this class.
     * @returns The declaring class signature.
     * @example
     * 1. get class signature from ArkMethod.

     ```typescript
     let methodSignature = expr.getMethodSignature();
     let name = methodSignature.getDeclaringClassSignature().getClassName();
     ```
     *
     */
    getDeclaringClassSignature(): ClassSignature;
    /**
     * Returns the sub-signature of this method signature.
     * The sub-signature is part of the method signature, which is used to
     * identify the name of the method, its parameters and the return value type.
     * @returns The sub-signature of this method signature.
     */
     getMethodSubSignature(): MethodSubSignature;
     getType(): Type;
     toString(): string;
     toMapKey(): string;
     isMatch(signature: MethodSignature): boolean;
     getParamLength(): number;
  }
```

```
export declare class MethodSubSignature {
    private methodName;
    private parameters;
    private returnType;
    private staticFlag;
    constructor(methodName: string, parameters: MethodParameter[], returnType: Type, staticFlag?: boolean);
    getMethodName(): string;
    getParameters(): MethodParameter[];
    getParameterTypes(): Type[];
    getReturnType(): Type;
    setReturnType(returnType: Type): void;
    isStatic(): boolean;
    toString(): string;
}
```

```
export declare class ClassSignature {
    private declaringFileSignature;
    private declaringNamespaceSignature;
    private className;
    static readonly DEFAULT: ClassSignature;
    constructor(className: string, declaringFileSignature: FileSignature, declaringNamespaceSignature?: NamespaceSignature | null);
    /**
     * Returns the declaring file signature.
     * @returns The declaring file signature.
     */
    getDeclaringFileSignature(): FileSignature;
    /**
     * Get the declaring namespace's signature.
     * @returns the declaring namespace's signature.
     */
    getDeclaringNamespaceSignature(): NamespaceSignature | null;
    /**
     * Get the **string** name of class from the the class signature. The default value is `""`.
     * @returns The name of this class.
     */
    getClassName(): string;
    /**
     *
     * @returns The name of the declare class.
     */
    getDeclaringClassName(): string;
    setClassName(className: string): void;
    getType(): ClassType;
    toString(): string;
    toMapKey(): string;
}
```

# dynEdges Format

I find the following useful information:

```
    getDynEdges(): Map<Method, Set<Method>>;
```

So the getDynEdges() API return a map.
