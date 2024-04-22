# semgrep-quickstart

Semgrep 是一个强大的开源静态分析工具，用于查找代码中的错误、检测安全漏洞以及强制执行代码标准。以下是一个基于 Semgrep 编写静态代码扫描规则的快速入门指南。

### 安装 Semgrep

首先，您需要在您的机器上安装 Semgrep。您可以通过多种方式安装，包括使用 pip 或 Homebrew：

```bash
# 使用 pip
pip install semgrep

# 使用 Homebrew
brew install semgrep
```

### 创建您的第一个规则

Semgrep 使用 YAML 文件定义规则。每个规则指定了要匹配的模式、当模式匹配时要显示的消息以及其他元数据。

1. **创建一个新的 YAML 文件**，例如 `my_first_rule.yml`，并打开它以编辑。
2. **添加以下内容**到文件中：

```yaml
rules:
- id: python-sql-injection
  patterns:
    - pattern: |
        $CURSOR.execute(..., f"SELECT ... FROM ... WHERE ... = '$USER_INPUT'...", ...)
    - pattern-not: |
        $CURSOR.execute(..., f"SELECT ... FROM ... WHERE ... = ?...", ...)
  message: "Potential SQL injection vulnerability. Use parameterized queries instead."
  languages: [python]
  severity: ERROR
```

然后写一个存在漏洞的python脚本，放到src/app.py里。

```

import sqlite3

def unsafe_query(database_path, user_input):
    conn = sqlite3.connect(database_path)
    cursor = conn.cursor()
    query = f"SELECT * FROM users WHERE username = '{user_input}'"
    cursor.execute(query)
    results = cursor.fetchall()
    cursor.close()
    conn.close()
    return results

# 示例用法
database_path = 'example.db'
user_input = input("Please enter your username: ")
results = unsafe_query(database_path, user_input)
print(results)

```



### 运行 Semgrep

使用以下命令运行 Semgrep 并指定您的规则文件：

```bash
semgrep scan my_first_rule.yml path_to_your_code_directory
```

这个命令会扫描指定路径下的代码，并应用您定义的规则。

### 使用 Semgrep 社区规则

Semgrep 拥有一个庞大的社区规则库，您可以直接引用这些规则而无需自己从头开始编写。访问 [Semgrep Registry](https://semgrep.dev/explore) 来浏览可用的规则和规则集。

### 集成到 CI/CD

Semgrep 可以集成到您的 CI/CD 流程中，以自动化代码审查和安全检查。您可以在 CI 配置文件中添加 Semgrep 命令，例如在 GitHub Actions 中：

```yaml
- name: Run Semgrep
  run: semgrep --config=p/r2c-ci --path=your_code_path
```

这样，每次提交代码时，Semgrep 都会自动运行并报告可能的问题。

通过以上步骤，您可以快速开始使用 Semgrep 编写和运行静态代码扫描规则。随着您对 Semgrep 的进一步了解，您可以探索更多高级功能和定制选项。

Citations:
[1] https://www.jit.io/blog/semgrep-rules-for-sast-scanning
[2] https://blogs.halodoc.io/streamlining-code-review-with-semgrep/
[3] https://www.youtube.com/watch?v=O5mh8j7-An8
[4] https://owasp.org/www-chapter-newcastle-uk/presentations/2021-02-23-semgrep.pdf
[5] https://parsiya.net/blog/2021-06-22-semgrep-the-surgical-static-analysis-tool/
[6] https://spaceraccoon.dev/comparing-rule-syntax-codeql-semgrep/
[7] https://semgrep.dev/docs/semgrep-code/semgrep-pro-engine-examples/
[8] https://semgrep.dev/docs/writing-rules/rule-syntax/
[9] https://semgrep.dev
[10] https://semgrep.dev/docs/writing-rules/pattern-examples/
[11] https://semgrep.dev/docs/supported-languages/
[12] https://semgrep.dev/docs/getting-started/
[13] https://discourse.julialang.org/t/static-analysis-with-semgrep/99251
[14] https://semgrep.dev/docs/semgrep-cloud-platform/github-pr-comments/
[15] https://notes.eatonphil.com/static-analysis-with-semgrep.html
[16] https://semgrep.dev/docs/deployment/core-deployment/
[17] https://semgrep.dev/docs/deployment/oss-deployment/
[18] https://deepsource.com/platform/static-analysis
[19] https://www.alchemy.com/dapps/semgrep
[20] https://github.com/semgrep/semgrep-vscode/blob/master/README.md
