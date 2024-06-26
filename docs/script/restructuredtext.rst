.. highlight:: rst
.. _restructuredtext:

=================
reStructuredText
=================

.. _indentation-rst:

缩进（Indentation）
-------------------

.. warning::

    - 本文档使用 4 格缩进，错误的缩进将导致无法正确渲染样式。
    - 如果在 Sphinx reST 内使用代码块，其缩进要求不受影响（如 Python）。

.. _sections-rst:

章节（Sections）
----------------

.. code-block::

    ========
    一级标题
    ========

    二级标题
    --------

    三级标题
    ~~~~~~~~

    四级标题
    ^^^^^^^^

.. warning::

    - **标记符必须与文本长度一致，否则会导致 Warning （无法通过 CI）。**
    - 你可以采用更深的嵌套级别，但在文档中应当避免出现四级甚至更深的标题。

.. card::

  正确示范
  ^^^^^^^^
  .. code-block::

      ========
      一级标题
      ========

  ---
  错误示范
  ^^^^^^^^
  .. code-block::

      ======================
      一级标题
      ======================

.. _paragraphs-rst:

段落（Paragraphs）
------------------

段落（:duref:`参考 <paragraphs>`）是由一个或多个空白行分隔的文本块，是 reST 文档中最基本的部分。
缩进在 reST 语法中很重要（与 Python 一样），因此同一段落的所有行都必须左对齐到相同的缩进级别。

保留换行特性
~~~~~~~~~~~~

行块（:duref:`参考 <line-blocks>`）是保留换行符的一种方法：

.. code-block::

    | 第一部分，
    | 第二部分。

| 第一部分，
| 第二部分。

.. _inlnie-markup-rst:

内联标记（Inline markup）
-------------------------

.. code-block:: text

    *一个星号标记出需要用户着重阅读的内容*

*一个星号标记出需要用户着重阅读的内容*

.. code-block:: text

    **两个星号表示文本十分重要，一般用粗体显示。**

**两个星号表示文本十分重要，一般用粗体显示。**

.. code-block:: text

    `一个反引号表示一个作品的引用，且必须包含作品的标题。`

`一个反引号表示一个作品的引用，且必须包含作品的标题。`

.. code-block:: text

    ``两个反引号表示预定义格式文本``

``两个反引号表示预定义格式文本``

.. dropdown:: 注意事项

    .. warning::

        标记符号与被包裹的文本内容之间不能存在空格，与外部文本之间必须存在空格。

    .. card::

      正确示范
      ^^^^^^^^
      .. code-block:: text

          这些文本 **表示强调** 作用

      这些文本 **表示强调** 作用
      ---
      错误示范
      ^^^^^^^^
      .. code-block:: text

          这些文本 ** 表示强调** 作用
          这些文本 **表示强调 ** 作用
          这些文本**表示强调** 作用

      这些文本 ** 表示强调** 作用
      这些文本 **表示强调 ** 作用
      这些文本**表示强调** 作用

.. _list-rst:

列表（List）
------------

.. warning::

    列表语法是最容易被用错的地方，在文档中也极为常见。

定义列表
~~~~~~~~

定义列表（:duref:`参考 <definition-lists>`）在 API 文档很常见，使用方法如下：

.. code-block::

    术语 （限定在一行文本）
        术语的定义，必须使用缩进。

        支持使用多个段落。

    下一个术语
        下一个术语对应的定义。

术语 （限定在一行文本）
    术语的定义，必须使用缩进。

    支持使用多个段落。

下一个术语
    下一个术语对应的定义。

无序列表
~~~~~~~~

无序列表（:duref:`参考 <bullet-lists>`）的用法很自然。
只需要在段落开头放置横杠，然后正确地缩进：

.. card:: 正确的示范（ **2 格缩进** ）

    .. code-block::

        - 这是一个无序列表。
        - 它有两个元素，
          第二个元素占据两行源码，视作同一个段落。

    - 这是一个无序列表。
    - 它有两个元素，
      第二个元素占据两行源码，视作同一个段落。

.. card:: 错误的示范（4 格缩进）

    .. code-block::

        - 这是一个无序列表。
        - 它有两个元素，
             第二个元素被解析成定义列表。

    - 这是一个无序列表。
    - 它有两个元素，
         第二个元素被解析成定义列表。

有序列表
~~~~~~~~

对于有序列表，可以自己编号，也可以使用 # 来自动编号：

.. code-block::

    1. 这是一个有序列表。
    2. 它也有两个元素。

1. 这是一个有序列表。
2. 它也有两个元素。

.. code-block::

    #. 这又是一个有序列表。
    #. 但是它能够自动编号。

#. 这又是一个有序列表。
#. 但是它能够自动编号。

嵌套列表
~~~~~~~~

嵌套列表必须使用 **空白行** 和父列表项目隔开：

.. card:: 正确示范（ **2 格缩进** ）

    .. code-block::

        - 这是一个列表。

          - 它嵌套了一个子列表，
          - 并且有自己的子元素。

        - 这里是父列表的后续元素。

    - 这是一个列表。

      - 它嵌套了一个子列表，
      - 并且有自己的子元素。

    - 这里是父列表的后续元素。

.. card:: 错误示范

    .. code-block::

        - 这并不是嵌套列表，
          - 前面三行被看作是同一个元素，
          - 其中横杠被解析成普通的文本。
        - 这是列表的第二个元素。

    - 这并不是嵌套列表，
      - 前面三行被看作是同一个元素，
      - 其中横杠被解析成普通的文本。
    - 这是列表的第二个元素。

.. _tables-rst:

表格（Tables）
--------------

网格表
~~~~~~

对于网格表（:duref:`参考 <grid-tables>`），必须手动 "画" 出单元格：

.. code-block::

    +------------------------+------------+----------+----------+
    | Header row, column 1   | Header 2   | Header 3 | Header 4 |
    | (header rows optional) |            |          |          |
    +========================+============+==========+==========+
    | body row 1, column 1   | column 2   | column 3 | column 4 |
    +------------------------+------------+----------+----------+
    | body row 2             | ...        | ...      |          |
    +------------------------+------------+----------+----------+

+------------------------+------------+----------+----------+
| Header row, column 1   | Header 2   | Header 3 | Header 4 |
| (header rows optional) |            |          |          |
+========================+============+==========+==========+
| body row 1, column 1   | column 2   | column 3 | column 4 |
+------------------------+------------+----------+----------+
| body row 2             | ...        | ...      |          |
+------------------------+------------+----------+----------+

简单表
~~~~~~

简单表（:duref:`参考 <simple-tables>`）写起来很简单，但有局限性：
它们必须包含多个行，并且第一列单元格不能包含多行。

.. code-block::

    =====  =====  =======
    A      B      A and B
    =====  =====  =======
    False  False  False
    True   False  False
    False  True   False
    True   True   True
    =====  =====  =======

=====  =====  =======
A      B      A and B
=====  =====  =======
False  False  False
True   False  False
False  True   False
True   True   True
=====  =====  =======

CSV 表
~~~~~~

CSV 表格可以根据 CSV（逗号分隔值）数据创建表。

.. code-block::

    .. csv-table:: Frozen Delights!
        :header: "Treat", "Quantity", "Description"
        :widths: 15, 10, 30

        "Albatross", 2.99, "On a stick!"
        "Crunchy Frog", 1.49, "If we took the bones out,
        it wouldn't becrunchy, now would it?"
        "Gannet Ripple", 1.99, "On a stick!"

.. csv-table:: Frozen Delights!
    :header: "Treat", "Quantity", "Description"
    :widths: 15, 10, 30

    "Albatross", 2.99, "On a stick!"
    "Crunchy Frog", 1.49, "If we took the bones out,
    it wouldn't becrunchy, now would it?"
    "Gannet Ripple", 1.99, "On a stick!"

List 表
~~~~~~~

List 表可以根据两级无序列表来生成表格：

.. code-block::

    .. list-table:: Frozen Delights!
        :widths: 15 10 30
        :header-rows: 1

        - - Treat
          - Quantity
          - Description
        - - Albatross
          - 2.99
          - On a stick!
        - - Crunchy Frog
          - 1.49
          - If we took the bones out, it wouldn't be
             crunchy, now would it?
        - - Gannet Ripple
          - 1.99
          - On a stick!

.. list-table:: Frozen Delights!
    :widths: 15 10 30
    :header-rows: 1

    - - Treat
      - Quantity
      - Description
    - - Albatross
      - 2.99
      - On a stick!
    - - Crunchy Frog
      - 1.49
      - If we took the bones out, it wouldn't be
         crunchy, now would it?
    - - Gannet Ripple
      - 1.99
      - On a stick!

.. _hyperlinks-rst:

超链接（Hyperlinks）
--------------------

使用 ```链接文本 <https://domain.invalid>`_`` 来插入内联网页链接。

你也可以使用目标定义（:duref:`参考 <hyperlink-targets>`）的形式分离文本和链接：

.. code-block::

    这个段落包含一个 `超链接`_.

    .. _超链接: https://domain.invalid/

这个段落包含一个 `超链接`_.

.. _超链接: https://domain.invalid/

.. warning::

    - 在链接文本和 ``<`` 符号之间必须至少有一个空格。
    - 同 :ref:`inlnie-markup-rst`，
      标记符和被包裹的文本之间不能有空格，
      而标记符和外部文本之间至少需要有一个空格。
    - 如果在同一个页面中两个 **链接文本** 相同，编译器会报 **警告**，
      此时，可以在末尾用两个下划线 ``__`` 来解决


.. _images-rst:

图片（Images）
--------------

reST 支持图像指令，用法如下：

.. code-block::

    .. image:: gnu.png
        :height: 100px (length)
        :width: 200px (length or percentage of the current line width)
        :scale: integer percentage (the "%" symbol is optional)
        :alt: alternate text
        :align: "top", "middle", "bottom", "left", "center", or "right"
        :target: text (URI or reference name)

    .. figure:: ../_static/images/lenna.jpg
        :height: 200px
        :width: 200px
        :alt: Lenna, 512 times 512, Grayscale
        :align: center
        :target: http://www.lenna.org/

        lenna.jpg

.. image:: ../_static/images/lenna.jpg
    :height: 200px
    :width: 200px
    :alt: Lenna, 512 times 512, Grayscale
    :align: center
    :target: http://www.lenna.org/

.. figure:: ../_static/images/lenna.jpg
    :height: 200px
    :width: 200px
    :alt: Lenna, 512 times 512, Grayscale
    :align: center
    :target: http://www.lenna.org/

    lenna.jpg

.. warning::

    - 文档中若包含 ``gif`` 或 ``svg`` 格式的图片将无法通过 XLaTeX 编译。解决方法是图片后缀名使用通配符 ``*``\。
    - ``figure`` 和 ``image`` 的区别在于， ``figure`` 可以添加图片标题，而 ``image`` 不能。
    - 文档中所使用的图片统一放在 ``source/_static/images`` 目录内。
    - 优先使用 ``svg`` 格式的矢量图或使用 :ref:`Mermaid <mermaid-ext>` 语法绘制示意图。

视频（Videos）
--------------

.. code-block:: html

    .. raw:: html

        <iframe width="560" height="315"
            src="//player.bilibili.com/player.html?aid=497651138&bvid=BV1BK411L7DJ&cid=177974677&p=1" scrolling="no" border="0"
            frameborder="no" framespacing="0" allowfullscreen="true">
        </iframe>

.. _cross-reference-rst:

交叉引用（Cross-reference）
---------------------------

使用 ``:role:`target``` 语法，就会创造一个 ``role`` 类型的指向 ``target`` 的链接。

- 最常见的使用情景是文档内部页面的相互引用（尤其是引用 API 参考内容时）。
- 显示的链接文本会和 ``target`` 一致，
  你也可以使用 ``:role:`title <target>``` 来将链接文本指定为 ``title``

.. _test-ref-label:

通过 ref 进行引用
~~~~~~~~~~~~~~~~~

为了支持对任意位置的交叉引用，使用了标准的 reST 标签（标签名称在整个文档中必须唯一）。

可以通过两种方式引用标签：

**1、** 在章节标题之前放置一个标签，引用时则可以使用 ``:ref:`label-name``` , 比如：

.. code-block::

    .. _test-ref-label:

    通过 ref 进行引用
    ~~~~~~~~~~~~~~~~~

    跳转到 :ref:`test-ref-label`

跳转到 :ref:`test-ref-label`。这种方法将自动获取章节标题作为链接文本，且对图片和表格也一样有效。

**2、** 如果标签没有放在标题之前，则需要使用 ``:ref:`Link title <label-name>``` 来指定名称。

.. _footnotes-rst:

脚注（Footnotes）
-----------------

脚注（:duref:`参考 <footnotes>`）使用 ``[#name]_`` 来标记脚注的位置，并在 ``Footnotes`` 专栏（rubic）后显示，例如：

.. code-block::

    Lorem ipsum [1]_ dolor sit amet ... [2]_

    .. rubric:: Footnotes

    .. [1] Text of the first footnote.
    .. [2] Text of the second footnote.

Lorem ipsum [1]_ dolor sit amet ... [2]_

.. rubric:: Footnotes

.. [1] Text of the first footnote.
.. [2] Text of the second footnote.

你可以显式使用 ``[1]_`` 来编号，否则使用 ``[#]_`` 进行自动编号。

.. _citation-rst:

引用（Citation）
----------------

引用和脚注类似，但不需要进行编号，且全局可用：

.. code-block::

    Lorem ipsum [Ref]_ dolor sit amet.

    .. [Ref] Book or article reference, URL or whatever.

Lorem ipsum [Ref]_ dolor sit amet.

.. [Ref] Book or article reference, URL or whatever.

.. _math-rst:

数学公式（Math）
----------------

只需要使用类似的语法：

.. code-block::

    Since Pythagoras, we know that :math:`a^2 + b^2 = c^2`.

Since Pythagoras, we know that :math:`a^2 + b^2 = c^2`.

.. _mermaid-ext:

Mermaid 语法支持
-----------------

文档已经通过 `sphinxcontrib-mermaid
<https://sphinxcontrib-mermaid-demo.readthedocs.io/en/latest/>`_ 插件支持
`Mermaid <https://mermaid-js.github.io/mermaid/>`_ 语法，样例如下：

.. code-block::

    .. mermaid::

        sequenceDiagram
            participant Alice
            participant Bob
            Alice->John: Hello John, how are you?
            loop Healthcheck
                 John->John: Fight against hypochondria
            end
            Note right of John: Rational thoughts <br/>prevail...
            John-->Alice: Great!
            John->Bob: How about you?
            Bob-->John: Jolly good!

.. mermaid::

    sequenceDiagram
        participant Alice
        participant Bob
        Alice->John: Hello John, how are you?
        loop Healthcheck
             John->John: Fight against hypochondria
        end
        Note right of John: Rational thoughts <br/>prevail...
        John-->Alice: Great!
        John->Bob: How about you?
        Bob-->John: Jolly good!

.. _toggle-ext:

Toggle 语法支持
---------------

文档已经通过 `sphinx-togglebutton
<https://sphinx-togglebutton.readthedocs.io/en/latest/>`_ 插件支持常见 Toggle 功能，样例如下：

.. code-block::

    .. admonition:: Here's my title
        :class: dropdown, warning

        My note

.. admonition:: Here's my title
    :class: dropdown, warning

    My note

以上展示的为基础用法，更多用法请参考文档。

.. _pannels-ext:

Pannels 语法支持
-----------------

文档已经通过 `sphinx-panels
<https://sphinx-panels.readthedocs.io/en/latest/>`_ 插件支持常见 Pannels 功能，样例如下：

.. code-block::

    .. card:: Card Title

        Header
        ^^^
        Card content
        +++
        Footer

    .. card:: Card Title

        Header
        ^^^
        Card content
        +++
        Footer

.. card:: Card Title

    Header
    ^^^
    Card content
    +++
    Footer

.. card:: Card Title

    Header
    ^^^
    Card content
    +++
    Footer


.. _tabs-ext:

Tabs 语法支持
--------------

文档已经通过 `sphinx-tabs <https://sphinx-design.readthedocs.io/en/sbt-theme/tabs.html>`_ 插件支持常见 Tabs 功能，样例如下：

.. code-block::

    .. tab-set::

        .. tab-item:: Apples

            Apples are green, or sometimes red.

        .. tab-item:: Pears

            Pears are green.

        .. tab-item:: Oranges

            Oranges are orange.

.. tab-set::

    .. tab-item:: Apples

        Apples are green, or sometimes red.

    .. tab-item:: Pears

        Pears are green.

    .. tab-item:: Oranges

        Oranges are orange.

以上展示的为 Basic 用法，Nested / Group / Code Tabs 用法请参考文档。

GitHub URL 缩写
----------------

为了方面写文档时引用 GitHub 上的源代码，支持如下语法：

.. code-block::

    - :src:`docs/`
    - :docs:`docs/conf.py`
    - :issue:`1`
    - :pull:`21`

- :src:`docs/`
- :docs:`docs/conf.py`
- :issue:`1`
- :pull:`21`

该功能通过 `sphinx.ext.extlinks
<https://www.sphinx-doc.org/en/master/usage/extensions/extlinks.html>`_ 插件支持。

参考文献
---------

基于 `sphinxcontrib-bibtex <https://sphinxcontrib-bibtex.readthedocs.io/en/latest/index.html>`_
插件书写参考文献。使用时首先将参考文献的引用写在 ``references.bib`` 中，然后在正文中添加引用。

引用出现的位置分为行内引用 ``cite`` 和脚注引用 ``footcite``，引用格式也分为引用时给出作者署名
``t`` 和引用时不给出作者署名，只在文中注明递增[序号] ``p``。因此其组合一共有四种：

1. ``:cite:t:``
2. ``:cite:p:``
3. ``:footcite:t:``
4. ``:footcite:p:``

对应地，插入参考文献可以使用 ``.. bibliography::`` 或 ``.. footbibliography::``。

将引用写入 references.bib
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: text

    @Book{1987:nelson,
        author = {Edward Nelson},
        title = {Radically Elementary Probability Theory},
        publisher = {Princeton University Press},
        year = {1987}
    }

行内引用
~~~~~~~~

.. code-block:: text

    See :cite:t:`1987:nelson` for an introduction to non-standard analysis.
    Non-standard analysis is fun :cite:p:`1987:nelson`.

    .. bibliography::

See :cite:t:`1987:nelson` for an introduction to non-standard analysis.
Non-standard analysis is fun :cite:p:`1987:nelson`.

.. bibliography::

.. warning::

    整个文档只能有一处写 ``.. bibliography::``，否则编译的时候会报重复引用的警告。如果只想在单个 ``rst`` 文件中写明参考文献，可以使用 ``footcite`` 来避免这种警告。

脚注引用
~~~~~~~~

.. code-block:: text

    See :footcite:t:`1987:nelson` for an introduction to non-standard analysis.
    Non-standard analysis is fun\ :footcite:p:`1987:nelson`.

    .. footbibliography::

See :footcite:t:`1987:nelson` for an introduction to non-standard analysis.
Non-standard analysis is fun\ :footcite:p:`1987:nelson`.

.. footbibliography::

.. note::

    使用反斜杠加空格来抑制脚注之前的空格。
