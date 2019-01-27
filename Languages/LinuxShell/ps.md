[Home](../)->[Linux命令](LinuxCommand)->ps命令
---
作用：列出活动的进程  
用法：ps [options]  
    a:
    u:显示以用户为导向的格式
    x:
    -A或-e:选择全部进程  
    -a:选择teminal的全部进程，但不包括session leaders  
    -d:选择全部进程，但不包括session leaders  
    -f:使用全格式监听
    -F:额外的全格式
    -l:长格式，常与-y一起使用。
    -y:不显示flags，显示rss而不是addr。只能与-l一起使用。  
    
示例：
    使用标准语法查看所有进程  
    `ps -e, ps -ef, ps -eF, ps -ely`
    使用BSD语法查看所有进程  
    `ps aux, ps ax`
