# Service中

service中的变量非必要不要用public，防止调用者任意修改

service中尽量使用直接继承extend单线解决，避免多次继承或者实例化，

