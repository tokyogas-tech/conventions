# Pythonコーディング規約

- [ruffの使用](#ruffの使用)
- [blackの使用](#blackの使用)
- [Docstringについて](#Docstringについて)
- [命名規則](#命名規則)
- [命名ガイド](#命名ガイド)
- [関数のパラメータ名の明示化](#関数のパラメータ名の明示化)
- [関数で多くのパラメータを扱う場合](#関数で多くのパラメータを扱う場合)
- [モジュールの読み込み](#モジュールの読み込み)
- [immutableタイプの使用](#immutableタイプの使用)
- [attrsの使用](#attrsの使用)
- [ファイル分割](#ファイル分割)

## ruffの使用

- 自身の使用しているエディタに [ruff](https://docs.astral.sh/ruff/) を使用する。
- [pre-commit](https://pre-commit.com/)、CIでもruffを使用し、何層かでLintする。

### 理由

- 命名規則の厳格化や、バグになりそうなコードの検出の為。
- 非常に高速なツールである為、開発の邪魔にもなり難いと考え、採用することとした。

### エディタの設定方法

- [PyCharmのruff設定](https://docs.astral.sh/ruff/editor-integrations/#pycharm-external-tool)
- [VSCodeのruff設定](https://docs.astral.sh/ruff/editor-integrations/#vs-code-official)
- [Vimのruff設定](https://docs.astral.sh/ruff/editor-integrations/#vim-neovim)

## blackの使用

- 自身の使用しているエディタに [black](https://black.readthedocs.io/en/stable/) を使用する。

### 理由

- 自動整形により、コードを綺麗に保つ。

### エディタの設定方法

- [PyCharmのblack設定](https://black.readthedocs.io/en/stable/integrations/editors.html#pycharm-intellij-idea)
- [VSCodeのblack設定](https://black.readthedocs.io/en/stable/integrations/editors.html#visual-studio-code)
- [Vimのblack設定](https://black.readthedocs.io/en/stable/integrations/editors.html#vim)

## Docstringについて

- [Googleスタイル](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html)を使用し、各関数に対して、ドキュメントを書くこと。

### 理由

- 各関数で行っていることを明文化し、パラメータや戻り値に対しても、どのようなことに使用されるのかを他者に理解してもらう為。

### エディタの設定方法

- [PyCharmでのDocstring自動生成](https://www.jetbrains.com/help/pycharm/creating-documentation-comments.html#create_pydoc)
- [VSCodeでのDocstring自動生成](https://marketplace.visualstudio.com/items?itemName=njpwerner.autodocstring)
- [VimでのDocstring自動生成](https://github.com/heavenshell/vim-pydocstring)

### 例

```python
def func(arg1, arg2):
    """概要

    詳細説明

    Arguments:
        パラメータ(arg1)の名前 (arg1の型): (arg1)の説明
        パラメータ(arg2)の名前 (arg2)の型, optional: (arg2)の説明

    Raises:
        例外の名前: 例外の説明

    Returns:
        戻り値の型: 戻り値の説明

    Yields:
        戻り値の型: 戻り値についての説明

    Examples:
        関数の使い方

        >>> func(5, 6)
        11

    Note:
        注意事項や注釈など
    """


return arg1 + arg2
```

## 命名規則

- 出来る限り分かり易い名称を使用し、クラス名、関数、変数名などは略語は避ける。
- ファイル名やフォルダ名は`_`で区切る。また小文字とする。
- クラス名はCamel Case、関数名はSnake Caseを使用する。
- 定数名は大文字とする。
- private関数・プロパティには先頭に`_`を付ける。
- 組込みの名称を避ける為に、末尾に`_`を付ける。
- testの関数名は、正常系は`test_*_happy_path(_with_something)`、異常系は`test_*_error(_with_something)`とする。
- mock名は、`mock_*`とする。

### 例

#### 略語を避ける

```python
cate = ""
```

上記の代わりに

```python
category = ""
```

#### ファイル名とフォルダ名について

```
- contenttypes
  - contenttypea.py
```

上記の代わりに

```
- content_types
  - content_type_a.py
```

#### クラス名と関数名について

```python
class foo_class:
    def DoSomething(self):
        ...
```

上記の代わりに

```python
class FooClass:
    def do_something(self):
        ...
```

#### 定数名について

```python
constant_name = ""
```

上記の代わりに

```python
CONSTANT_NAME = ""
```

#### privateについて

```python
class FooClass:
    def do_something(self):
        ...
```

上記の代わりに

```python
class FooClass:
    def _do_something(self):
        ...
```

#### 組込みの名称を使用する場合

```python
property = "something"
```

上記の代わりに

```python
property_ = "something"
```

#### testの関数名について

```python
# 正常時
def test_something_happy_path():
    ...


# 異常時
def test_something_error_with_string_param():
    ...


def test_something_error_with_date_param():
    ...
```

#### mock名について

```python
from unittest import mock


@mock.patch(
    "something.doing"
)
def test_something(doing_mock):
    ...
```

上記の代わりに

```python
from unittest import mock


@mock.patch(
    "something.doing"
)
def test_something(mock_doing):
    ...
```

## 命名ガイド

- 型情報を関数名や変数名には含めない。
- 以下を参考にすると良い。
  - [https://qiita.com/YutaManaka/items/62dda256bb7ba6c08399](https://qiita.com/YutaManaka/items/62dda256bb7ba6c08399)
  - [https://zenn.dev/chelproc/articles/4f1da698779f67](https://zenn.dev/chelproc/articles/4f1da698779f67)

### 例

- bool型

```python
enabled = True
cached = False
```

上記の代わりに

```python
is_enabled = True
has_cached = False
```

- collection型

```python
account_list = []
```

上記の代わりに

```python
accounts = []
```

## 関数のパラメータ名の明示化

- 出来る限り、関数には`*args`と`**kwargs`の使用を避け、パラメータ名を明示する。
- パラメータは優先度が高いものから並べると良い。
- `*`を使用し、関数呼び出し側の引数名の使用を強制した方がより良い。

### 理由

- 関数に送られる変数名を明確にする為。

### 例

```python
def do_something(**kwargs) -> None:
    ...
```

上記の代わりに

```python
def do_something(*, foo: int, bar: str) -> None:
    ...
```

## 関数で多くのパラメータを扱う場合

- 関数で多くのパラメータを扱う場合には、データオブジェクトを受け渡すことを検討する。
- 以下のように多くのパラメータがある場合も、`**kwargs`の使用は出来るだけ避ける。

### 理由

- 関数に送られる値が、どのパラメータに対してのものなのか不明確になってしまう為。
- 冗長になってしまうことを避ける為、データオブジェクトなどを受け渡す。

### 例

```python
def main():
    do_something(
        one=1, two=2, three=3, four=4, five=5
    )


def do_something(**kwargs):
    _execute(**kwargs)
```

上記の代わりに

```python
import attrs


@attrs.define
class FooClass:
    one: int
    two: int
    three: int
    four: int
    five: int


def main():
    foo = FooClass(
        one=1,
        two=2,
        three=3,
        four=4,
        five=5
    )
    do_something(
        foo=foo
    )


def do_something(*, foo: FooClass):
    _execute(foo=foo)
```

## モジュールの読み込み

- モジュールの読み込みは、オブジェクトではなく、モジュール名を指定する。
- ワイルドカード(`*`)によるimportは行わない。
- 以下のような場合には、`HttpResponse`のようなオブジェクトを指定するのではなく、`HttpResponse`を持つ`http`のようなモジュールを指定する。

### 理由

- モジュールの名前空間を簡潔にさせ、偶発的な衝突を少なくする為。

### 例

```python
from django.http import HttpResponse, HttpResponseRedirect, HttpResponseBadRequest
from django.shortcuts import render, get_object_or_404
```

上記の代わりに

```python
from django import http, shortcuts
```

## immutableタイプの使用

- immutableタイプの使用には多くのメリットがあるので、データの変更がない場合には、immutableタイプを使用すること。

### 理由

- 開発者がコードを理解しやすくなる。
- メモリ消費量を抑えることが出来る。
- setsに格納することが出来る。

### 例

- `@attrs.define`の代わりに`@attrs.frozen`を使用する。
- `set`の代わりに`frozenset`を使用する。
- `list[object]`の代わりに`tuple[object, ...]`または`collections.abc.Sequence[object]`を使用する。

## attrsの使用

- dataclassesかattrsかの選択に迷った場合は、attrsの使用を検討すること。

### 理由

- attrsは、dataclassesとほぼ互換性があり、より多くの機能を要している為。
- デフォルトで [\__slots__](https://wiki.python.org/moin/UsingSlots) を使用しており、メモリ消費量を抑え、パフォーマンスの向上が見込める為。

### 例

```python
import attrs


@attrs.frozen
class FooClass:
    value: int
```

## ファイル分割

- 一つのファイルが大きくなってしまった場合、新規フォルダ + `_file.py` + `__init__.py`で大きなファイルを細分化する。

### 理由

- 視認性・検索性の低下を防ぐ。
- `__init__.py`によって、モジュールの呼び出しをまとめられるので、呼び出し元からは一つに見せることが出来る。

### 例

```
- something.py
```

上記の代わりに

```
- something
  - __init__.py
  - _foo.py
  - _bar.py
```

\_\_init_\_\.py

```python
from _foo import is_foo
from _bar import get_bar
```