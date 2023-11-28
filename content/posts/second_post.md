+++
title = 'Second_post'
date = 2023-11-28T16:12:12+08:00
draft = false

author = "youyuan"

+++

# Flask 快速入门指南

## 开启调试模式
```python
flask --app hello run --debug
```

## 路由
使用 `route()` 装饰器把函数绑定到 URL。路由用于将请求的 URL 映射到具体的处理函数。

### 基本路由
```python
@app.route('/')
def index():
    return 'Index Page'

@app.route('/hello')
def hello():
    return 'Hello, World'
```

### 动态路由
动态变化 URL 的某些部分。
```python
@app.route('/user/<username>')
def show_user_profile(username):
    return f'User {escape(username)}'

@app.route('/post/<int:post_id>')
def show_post(post_id):
    return f'Post {post_id}'
```

### 唯一的 URL/重定向行为
```python
@app.route('/projects/')
def projects():
    return 'The project page'

@app.route('/about')
def about():
    return 'The about page'
```

## 使用 url_for 构建 URL
```python
@app.route('/hello')
def hello():
    return 'Hello, World'
    
@app.route('/about')
def about():
    print('/hello')
    print(url_for('hello'))
    return 'The about page'
```

### 动态调整路由
```python
@app.route('/hello_world')
def hello():
    return 'Hello, World'
```

## 路由映射到同一个视图函数
```python
@app.route('/hello/')
@app.route('/hello/<name>')
def hello(name=None):
    return render_template('hello.html', name=name)
```

### Jinja2 模板示例
```html
<body>
{% if name %}
    <h1>Hello {{ name }}!</h1>
{% else %}
    <h1>Hello, World!</h1>
{% endif %}
</body>
```

## 操作请求数据
```python
@app.route('/hello/')
@app.route('/hello/<name>')
def hello(name=None):
    print('请求路径：	', request.path)
    print('请求方法：	', request.method)
    print(dir(request))
    return render_template('hello.html', name=name)
```

### 登录处理
```python
@app.route('/login', methods=['POST', 'GET'])
def login():
    error = None
    if request.method == 'POST':
        print('username:	', request.form['username'])
        print('password:	', request.form['password'])
        return {'status': True, 'message': '登陆成功'}
    return render_template('login.html', error=error)
```
