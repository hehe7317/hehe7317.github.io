# hehe7317.github.io
个人博客

赖，只在编译和测试时有效，依赖不被传递
  - **runtime**：此依赖只在运行或测试时有效，不参与编译，依赖被传递
  - **test**：此依赖只在测试的编译和运行期时有效，依赖不被传递
  - **system**：见 [下文system](#systemScope)
  - **import**：此 scope 只用于 packaging 为 pom 的 dependencyManagement 节点下，用于伪实现 多父项目继承(maven 只支持单父项目)
- 以下表格，左边列表是当前项目的依赖，上面行是当前依赖所依赖的依赖

|          | compile  | provided | runtime  | test |
| -------- | -------- | -------- | -------- | ---- |
| compile  | compile  | -        | runtime  | -    |
| provided | provided | -        | provided | -    |
| runtime  | runtime  | -        | runtime  | -    |
| test     | test     | -        | test     | -    |

- 有上表可以得出以下结论
  - 被传递的依赖如果是 complie 类型，它会随着本项目的依赖的 scope 而相应变化（取最小作用域的那个 scope）
  - provided、test 不会被传递
  - 被传递的依赖如果是 runtime 类型，会综合本项目依赖的 scope 取最小作用域的那个 scope

###  *Dependency Management*

- 依赖项管理是一种集中依赖项信息的机制
- 如果有很多项目继承与一个共同的父项目，可以将所有依赖信息放入父 pom，并且在子 pom 中简单的引用父 pom
- dependencyManagement 中的依赖优先于 依赖就近原则

###  <a id="systemScope">system scope</a>

- 在当前系统路径中找依赖(而不是在仓库中)。一般为 JDK 或 VM 提供的依赖

```xml
<project>
  ...
  <dependencies>
    <dependency>
      <groupId>javax.sql</groupId>
      <artifactId>jdbc-stdext</artifactId>
      <version>2.0</version>
      <scope>system</scope>
      <systemPath>${java.home}/lib/rt.jar</systemPath>
    </dependency>
    <dependency>
      <groupId>sun.jdk</groupId>
      <artifactId>tools</artifactId>
      <version>1.5.0</version>
      <scope>system</scope>
      <systemPath>${java.home}/../lib/tools.jar</systemPath>
    </dependency>
  </dependencies>
  ...
</project>
```
