tmux的简单使用
============

## 新建会话

```bash
$ tmux new -s test
```

## 分屏: 同window多panel

```bash
Prefix = <Ctrl+b>

Prefix + %	split-window -h (split horizontally)
Prefix + "	split-window -v (split vertically)
```

## 新建window以及window之间的切换

```bash
Prefix + C

Traversing windows
Prefix + p and Prefix + n
```

## 新建新会话

```bash
Prefix + :new -s django
```

## 会话之间的切换

```bash
Prefix + s
```