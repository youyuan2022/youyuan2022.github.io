+++
title = 'Second_post'
date = 2023-11-28T16:12:12+08:00
draft = false

author = "youyuan"

+++

# Flask ��������ָ��

## ��������ģʽ
```python
flask --app hello run --debug
```

## ·��
ʹ�� `route()` װ�����Ѻ����󶨵� URL��·�����ڽ������ URL ӳ�䵽����Ĵ�������

### ����·��
```python
@app.route('/')
def index():
    return 'Index Page'

@app.route('/hello')
def hello():
    return 'Hello, World'
```

### ��̬·��
��̬�仯 URL ��ĳЩ���֡�
```python
@app.route('/user/<username>')
def show_user_profile(username):
    return f'User {escape(username)}'

@app.route('/post/<int:post_id>')
def show_post(post_id):
    return f'Post {post_id}'
```

### Ψһ�� URL/�ض�����Ϊ
```python
@app.route('/projects/')
def projects():
    return 'The project page'

@app.route('/about')
def about():
    return 'The about page'
```

## ʹ�� url_for ���� URL
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

### ��̬����·��
```python
@app.route('/hello_world')
def hello():
    return 'Hello, World'
```

## ·��ӳ�䵽ͬһ����ͼ����
```python
@app.route('/hello/')
@app.route('/hello/<name>')
def hello(name=None):
    return render_template('hello.html', name=name)
```

### Jinja2 ģ��ʾ��
```html
<body>
{% if name %}
    <h1>Hello {{ name }}!</h1>
{% else %}
    <h1>Hello, World!</h1>
{% endif %}
</body>
```

## ������������
```python
@app.route('/hello/')
@app.route('/hello/<name>')
def hello(name=None):
    print('����·����	', request.path)
    print('���󷽷���	', request.method)
    print(dir(request))
    return render_template('hello.html', name=name)
```

### ��¼����
```python
@app.route('/login', methods=['POST', 'GET'])
def login():
    error = None
    if request.method == 'POST':
        print('username:	', request.form['username'])
        print('password:	', request.form['password'])
        return {'status': True, 'message': '��½�ɹ�'}
    return render_template('login.html', error=error)
```
