# 网站应用结构图

```flow
open=>start: Open:>https://github.com/knsv/mermaid
    userInput=>inputoutput: User Input
    processes=>operation: Processes
    results=>condition: Yes or No?
    regis=>condition: GoSignUp,Yes or No?
    userr=>operation: SignUp
    end=>end: End
    open->userInput->processes->results
    results(yes)->end
    results(no)->regis
    regis(yes)->userr
    regis(no)->userInput
```

