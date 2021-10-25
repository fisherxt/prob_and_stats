[TOC]

```plain
author { zhch }
email { fisherxt@gmail.com }
github { github.com/fisherxt }
date { 2021-10-20 }

compiling passed env {
 - macOS 11.6 ( Arm_M1_Chip )
 - MacTeX / TeX Live 2021
 - XeLaTeX engine
 }
```

### Updates and Bug fixes

- 1021

  - { bugfix }  `chapintro` 修复文本高度变化时，右侧小图整体应未与边框底部对齐。

  - { bugfix } 章节为两位数时，标题中章节数字显示效果不居中。

  - { update } 封面英文书名字母间距([TeX.SE](https://tex.stackexchange.com/questions/82887/xelatex-space-between-letters))

    ```latex
    % Based on example 18 from the fontspec documentation
    {\tamil\addfontfeature{LetterSpace=8.0} Probability and Statistics}
    ```

  - { update } \<ascii\> 字体与 \<CJK\> 一致。

  - { update } 优化无衬线字体设置。

    - 定义 `\ltseries` 和 `\ebseries` 表示细体和极粗体。

      ```latex
      \setCJKsansfont{Sarasa Gothic J}[
      FontFace = {lt}{n}{* Light},
      UprightFont     = *,
      BoldFont        = * Bold,
      ItalicFont      = * Italic,
      BoldItalicFont  = * Bold Italic,
      FontFace = {eb}{n}{Font = * Bold, FakeBold=1.4}]
      %
      \DeclareRobustCommand{\ltseries}{\fontseries{lt}\selectfont}
      \DeclareRobustCommand{\ebseries}{\fontseries{eb}\selectfont}
      ```

  - { update } `whosays` 环境，右侧框线非全黑，已添加灰度。

  - { update } 增加测试内容。

  - { update } 目录

    - `\backmatter` 后章条目格式改变

      ```latex
      % Restore chapter entry contents line
      \titlecontents{chapter}
      [0pt]
      {\addvspace{15pt}\sffamily\small}
      {}
      {}
      {\titlerule*[3pt]{.}\contentspage}
      [\addvspace{-15pt}\nointerlineskip]
      
      % -- 另一种实现
      \dottedcontents{chapter}
      [0pt]
      {\addvspace{15pt}\sffamily\small}
      {0pt}
      {3pt}
      [\addvspace{-15pt}\nointerlineskip]
      ```

    - 目录条目不在同一章内换页

      [`\vspace{-\baselineskip}\filbreak`](https://tex.stackexchange.com/questions/56100/suppress-pagebreaks-in-titletocs-titlecontents-blocks)

      ```latex
      \titlecontents{chapter}
      [\indent@chapter@entry@toc]
      {\addvspace{15pt}\sffamily\bfseries\normalsizer}
      {\vspace{-\baselineskip}\filbreak\contentslabel{\indent@chapter@entry@toc}\large}
      {\hspace*{-\indent@chapter@entry@toc}\sffamily\small}
      {}
      ```

    - 调整目录页码数字字号为`\normalsize`

    - 目录点线长度适应页码数字宽度

### 基础配置

- 使用`UTF-8`编码，`XeLaTeX`引擎。

```latex
% !TeX encoding = UTF-8
% !TeX program = xelatex
```

- 行距1.4。

- 缩进1个全角字符宽度。

```latex
\documentclass[%
    UTF8, 
    zihao=false, 
    scheme=plain,
    linespread=1.4, 
    autoindent=1em]{ctexbook}
```

- 默认字号由`fontsize`宏包指定。

```latex
\RequirePackage { fontsize }
\changefontsize[11]{9.25} 
```

### 页面尺寸

标准的`A5`尺寸，`148mm x 210mm`。

使用 `geometry` 宏包指定。

```latex
\RequirePackage [%
    hmargin={18.5mm,16.5mm},
    vmargin={0.68in,0.72in},
    footskip=40pt,
    paper=a5paper,
%    layout={5.8in,8.3in},
%    layoutoffset=1.0in,
%    showframe, 
%    showcrop
]{geometry}
```

### 字体

开源字体近似替换商用字体。

- 正文明朝体，接近于[UD Reimin 黎明体](https://tw.morisawa.co.jp/fonts/specimen/6122)，以思源宋体替换。

- 无衬线哥特体，接近于[UD ShinGo 新黑体](https://tw.morisawa.co.jp/fonts/specimen/6138)/[UD ShinGo 新黑圆体](https://tw.morisawa.co.jp/fonts/specimen/6158)，以纱更黑体替换。

- 章节数字，接近于[UD Typos 512](https://www.typebank.co.jp/fontfamily/udtypos/)，以无衬线代替（相似度较差）。

- 封面标题使用加粗粗体无衬线，用 `AutoFakeBold=1.4` 近似。

- 封面文字、章节标题、页眉使用了细体哥特体，额外定义 `\sflight`。

- 页码数字使用 [Latin 725](https://www.fontshop.com/families/latin-725) 字体 `\latin`。（商用字体，可免费下载）。

- 两种封面英文字体 `\xkcd` 和 `\tamil` (只用到英文字形，不涉及泰米尔文字形)，字体文件位于`./fonts/`路径。

```latex
\RequirePackage { fontspec }
\RequirePackage { fontawesome }
\setCJKmainfont{Source Han Serif}[
    UprightFont     = * Medium,
    BoldFont        = * Heavy,
    ItalicFont      = * SemiBold,
    BoldItalicFont  = * SemiBold]
\setCJKsansfont{Sarasa Gothic J}[
    UprightFont     = *,
    BoldFont        = * Bold,
    ItalicFont      = * Italic,
    BoldItalicFont  = * Bold Italic]
\setCJKmonofont{Sarasa Mono SC}
%
\newCJKfontfamily{\sflight}{Sarasa Gothic J Light}[
    BoldFont        = Sarasa Gothic J Semibold]
\newCJKfontfamily{\sfbold}{Sarasa Gothic J Bold}[AutoFakeBold=1.4]
\newfontfamily{\latin}{Latin725BT-Roman.otf}[Path=./fonts/]
\newfontfamily{\xkcd}{xkcd-script.ttf}[Path=./fonts/, Ligatures=TeX]
\newfontfamily{\tamil}{Tamil MN.ttc}[Path=./fonts/, Ligatures=TeX]
```

- `fontawesome` 字体

  章末习题使用了`fontawesome`中的钻石形。

  `fontawesome` 宏包的字体定义 [`\newfontfamily{\FA}{FontAwesome}`](https://tex.stackexchange.com/questions/132888/fontawesome-font-not-found) 未指定路径，常找不到字体。

  复制了`fontawesome.otf` 到主目录，无需再修改`.sty`或额外指定字体路径。

  列表环境和正文用到加圈数字，使用 `TikZ` 定制了数字加圈的宏。

```latex
% -- circled numbers
\newcommand*\circled[1]{%
	\tikz[baseline=(char.base)]{
  \node[shape=circle,draw,inner sep=1pt] (char) {\scriptsize#1}; } }
```

### 标题格式

#### 章标题

- 无数字章节（引言章和目录）。标题居中、无衬线字体。
- 标准章节。首尾粗横线。`chapter/number` 细无衬线体，和 `chapter/name` 竖向中心对齐，加下划线，`chapter/title` 常规无衬线体。

#### 节标题

- 常规无衬线体。

#### 条标题

- 加粗无衬线体。

```latex
\newcommand\chapnumber[1]{\raisebox{-.5ex}[1ex][.9ex]{\makebox[2.3ex]{\textbf{#1}}}}
\newcommand\chapname[1]{\underline{\Large#1}}
\newcommand\secnumber[1]{\raisebox{-.2ex}{\textbf{#1}}}
\ctexset{%
    tocdepth = section,
    chapter = {
        beforeskip = \CTEXifname{-22pt}{-12pt},
        afterskip = 25pt,
        format = \CTEXifname{\sflight\centering}{\bfseries\sffamily\raggedright},
        name = {第,章},
        nameformat = \rule{\linewidth}{.7pt}\par\vspace{.22in}\chapname,
        number = \arabic{chapter},
        numberformat = \zihao{0}\chapnumber,
        titleformat = \CTEXifname{\huger}{\LARGE},
        aftername = \par\nointerlineskip\vspace{.36in},
        aftertitle = \par\nointerlineskip\vspace{0.3in}\CTEXifname{\vspace{0.2in}\rule{\linewidth}{.7pt}}{}\par\nointerlineskip,
        afterindent = true
    },
    section = {
        format = \sffamily\LARGE\raggedright,
        numberformat = \huge\secnumber,
        aftername = \hspace{1.2em},
        beforeskip = 22pt,
        afterskip = 8pt,
        aftertitle = \par,
        afterindent = true
    },
    subsection = {
        format = \sffamily\bfseries\zihao{5}\raggedright,
        number = (\arabic{subsection}),
        numberformat = \sffamily,
        afterskip = 0pt,
        afterindent = true
    }
}
```

### 特殊格式

- 强调格式

  - 下划线 `\uemph`。

    划线在标点符号处无中断。

  - 加粗 `\bemph`。

    **⚠️BUG**：`\bemph` 与 `\uemph` 同时使用会出现只加粗首字的情况。

    **临时方案**：用盒子包裹`\mbox{}`。

- ruby 注音。

- 波浪连接符 `\tildeto`。

- 无衬线字形引用（基于`cleveref`宏包`\cref`命令) `\sfcref`。

  ```latex
  \RequirePackage{%
      xeCJKfntef,  % -- emphasis
      pxrubrica    % -- ruby
  }
  \xeCJKsetup { underline = { skip = false } }
  \newcommand\uemph[1]{\CJKunderline{#1}}
  \newcommand\bemph[1]{\textsf{\textbf{#1}}}
  \newcommand\tildeto{~～~}
  \newcommand\sfcref[1]{\textsf{\cref{#1}}}
  ```

### 页眉页脚

#### `plain` 风格

- 页脚外侧放置页码。

#### `headings` 风格。

- 页脚外侧放置页码。
- 奇数页眉。右侧节标题。
- 偶数页眉。左侧章标题。

```latex
\RequirePackage{fancyhdr}
\fancypagestyle{this@headings}{%
    \fancyhead{}
    \fancyhead[EL]{\sflight\small\leftmark}
    \fancyhead[OR]{\sflight\small\rightmark}
    \fancyfoot{}
    \fancyfoot[EL,OR]{\small\latin\thepage}
    \renewcommand{\headrulewidth}{\z@}
    \renewcommand{\footrulewidth}{\z@}}
\renewcommand\frontmatter{%
    \if@openright\cleardoublepage\else\clearpage\fi
    \@mainmatterfalse
    \pagenumbering{roman}
    \pagestyle{this@headings}}
\renewcommand\mainmatter{%
    \if@openright\cleardoublepage\else\clearpage\fi
    \@mainmattertrue
    \pagenumbering{arabic}
    \pagestyle{this@headings}}
\renewcommand\backmatter{%
    \if@openright\cleardoublepage\else\clearpage\fi
    \@mainmatterfalse
    \pagestyle{this@headings}}
% - apply pagestyle to special page(chap 1st page, etc...)
\renewcommand\ps@plain{%
    \fancyhead{}
    \fancyfoot[EL,OR]{\small\latin\thepage}}
```

### 文本框环境

#### 章前引言 Box

- 切角矩形，行内居中对齐。

- 右侧小图堆叠。

  `TikZ/shapes.misc` 中预设 `chamfered rectangle` 形状，容易实现边框，但不易控制文字边距。

  后用 `tcolorbox` 绘制，容易控制文字位置。 

  细节：右侧小图堆叠，重叠边缘有阴影，暂未实现。

  ```latex
  % -- chapter intro box
  \newtcolorbox{chapintro}{
      enhanced,
      boxsep=0pt,
      left=1.5em,right=1.5em,top=1.5em,bottom=1.5em,
      width=.9\textwidth,
      before upper={\parindent1em},
      fontupper=\sffamily\footnotesizer,
      rightupper=1.25cm,
      center,frame hidden,
      interior code={
          \draw[line width=0.3pt] 
          (frame.south west) -- 
          ([yshift=-4mm]frame.north west) -- 
          ([xshift=4mm]frame.north west) -- 
          (frame.north east) -- 
          ([yshift=4mm]frame.south east) -- 
          ([xshift=-4mm]frame.south east) -- 
          cycle; 
      },
      overlay={
          \node[anchor=center, rotate=20] 
          at ([xshift=2mm,yshift=6.5mm]frame.south east)
          {\includegraphics[width=14mm,height=9mm]{example-image-a}};
          \node[anchor=center, rotate=-15] 
          at ([xshift=-1.5mm,yshift=16.5mm]frame.south east)
          {\includegraphics[width=14mm,height=8.5mm]{example-image-a}};
          \node[anchor=center, rotate=20] 
          at ([xshift=0pt,yshift=25.5mm]frame.south east)
          {\includegraphics[width=12mm,height=9mm]{example-image-a}};
          \node[anchor=center, rotate=15] 
          at ([xshift=-6.5mm,yshift=0mm]frame.south east)
          {\includegraphics[width=9mm,height=9mm]{example-image-a}};
      }
  }
  ```
  
  

#### 類題 Box

常规 `tcolorbox` ，需建立计数器。

```latex
% -- 類題 question box
\newcounter{sim}[chapter]
\newtcolorbox[use counter=sim]{simQ}{%
    enhanced,breakable,
    title={類題~\thechapter.\thetcbcounter},
    fonttitle=\bfseries\rmfamily\footnotesize,
    left=.5em,right=.5em,top=3pt,bottom=4pt,boxsep=2pt,
    before skip=10pt,after skip=12pt,
    colback=black!10!white,
    colframe=black!50!white,
    width=\textwidth,
    boxrule=0.5pt,
    before upper={\parindent1em},
    fontupper=\normalsize,
    sharp corners
}
```

#### 答え Box

常规盒子，定制框线。

```latex
\newtcolorbox{simA}{%
    enhanced,
    breakable,
    borderline west={3pt}{0pt}{black!20!white},
    colbacktitle=black!62!white,
    title={答え},
    fonttitle=\sffamily\bfseries\footnotesize,
    attach boxed title to top left={yshift=-2.75pt,xshift=-1.225pt},
    %    boxed title size=copy,
    %    toptitle=0pt,
    %    bottomtitle=0pt,
    right=0pt,boxsep=0pt,
    before skip=10pt,after skip=10pt,
    colback=white,
    frame hidden,
    before upper={\parindent1em},
    boxed title style={frame hidden, 
        left=4pt,right=4pt,top=1.75pt,bottom=1.75pt,boxsep=0mm}
}
```

#### 人物会话

- 命令格式：`\whosays{<name>}{<words>}`

- 实现：

  需预设熊教授和萝莉头像的图名。

  `TikZ` 绘图，左侧 `node` 放置头像，右侧 `shapes.callouts` 图形。

  **？问题**：设置圆角矩形的 `callout` 形状为圆角时，突出的尖角会变圆角，无预设选项可以消除。

  **！解决**：隐藏  `callout` 形状框线，路径绘制框线，实现尖角。[TeX.SE](https://tex.stackexchange.com/questions/619450/tikz-callouts-how-to-prevent-sharp-pointer-tip-from-being-rounded)

```latex
% -- 预设图名
\def\bear{example-image-a}
\def\loli{example-image-b}
\newcommand{\whosays}[2]{%
    \ifthenelse { \equal{#1}{bear} }{%
        \edef\img{ \bear }
        }{%
        \ifthenelse { \equal{#1}{loli} }{%
            \edef\img{ \loli }
        }{%
            \def\img{ example-image-a }
        }
    }
    \smallskip
    \noindent
    \begin{tikzpicture}
        \node [anchor=south west,inner sep=0pt] at (0, 0) (cartoon) {\includegraphics[width=.188\textwidth,height=.2\textwidth]{\img}};
        \node [%
        anchor=north west,
        rectangle callout,
        callout absolute pointer=(cartoon.center), 
        callout pointer width=0.34cm,
        draw=none,
        rounded corners=3pt,
        text width=0.73\textwidth, 
        minimum width=0.72\textwidth, 
        inner sep=2ex] at (.21\textwidth,.16\textwidth)
        (words) 
        {\small\sflight #2};
        
        \draw[rounded corners=3pt] (words.north west) -- (words.north east) -- (words.south east) -- (words.south west) to[sharp corners]
        ([yshift=-5mm]words.north west) to[sharp corners]
        ([xshift=-1.5mm, yshift=-3.5mm]words.north west) to[sharp corners]
        ([yshift=-2mm]words.north west) -- cycle
        ;
    \end{tikzpicture}
    \smallskip
}
```

#### 例题 Box

需定义计数器。

```latex
\newcounter{example}[chapter]
\newcommand{\extitle}{%
    \stepcounter{example}
    \tcbox[%
    enhanced,rounded corners,frame empty,
    left=4pt,right=4pt,top=1.75pt,bottom=1.75pt,boxsep=0pt,
    arc=5pt,auto outer arc,
    coltext=black,
    colback=black!20!white,
    fontupper=\sffamily\bfseries\footnotesize,
    before skip=5pt,after skip=5pt,
    ]{例~\thechapter.\arabic{example}}\par
}
```

### 脚注格式

- 脚注标记数字前有 `*` 和显著空白。
- 页脚内标记为正常字体，无上标。
- 脚注内容较长时，后行缩进跟首行首字对齐。

```latex
% ==== Footnote style
\let\@makefntextorig\@makefntext
\newlength{\fntnumberwidth}
\setlength{\fntnumberwidth}{2.25em}
\newcommand{\@makefntextcustom}[1]{%
    \makebox[\fntnumberwidth][l]{\thefootnote\hspace{\fntnumberwidth}}
    \parbox[t][0pt]{\textwidth-\fntnumberwidth}{#1}%
}
\renewcommand{\@makefntext}[1]{\@makefntextcustom{#1}}
\renewcommand{\thefootnote}{\sffamily *~\arabic{footnote}}
%
```

### 章末习题环境

- 标题。
  - 基于 `tcolorbox` 定制。
  - 列入目录，级别为`section`。
- 题目使用`enumitem`定制的列表环境。
  - 题号数字与左边距线对齐。
  - 题号数字具有固定宽度，内容缩进不因题号长度改变。
  - 题号左侧具有钻石符号。
    - 钻石分为三级，用 `\diamonds{<1,2,3>}` 来指定。
    - 多个钻石上下排列无竖向间隙。
    - 钻石位于 `margin` 区域。
    - 钻石符号顶部与题号顶部对齐。
- 题目内部会嵌套列表环境。
  - 全部题目具有一致格式。

```latex
% ==== Exercise Section Customization
% -- section title box
\newcommand{\exersection}{%
    \begin{tcolorbox}[
        before skip=15pt,
        after skip=6pt,
        text width=\textwidth,
        sharp corners,
        halign=flush center,
        colback=black!50!white,
        coltext=white,
        left=0pt,right=0pt,top=3pt,bottom=3pt,
        boxsep=0pt,
        boxrule=0pt,
        toprule=1.25pt
        ]
        \makebox[6em][s]{\sffamily\bfseries 章末問題}
    \end{tcolorbox}
    \addcontentsline{toc}{section}{章末問題}
}
% -- Diamonds
\newcommand{\points}[1]{% Print points in margin
    \leavevmode\makebox[0pt][r]{#1\hspace{2.5em}}\ignorespaces}
\newcommand\diamonds[1]{
    \ifnum#1=1
        \points{\parbox[t][0pt][t]{1em}{\scriptsize\faDiamond}}
    \else
        \ifnum#1=2
            \points{\parbox[t][0pt][t]{1em}{\scriptsize\faDiamond\par\nointerlineskip\faDiamond}}
        \else
            \points{\parbox[t][0pt][t]{1em}{\scriptsize\faDiamond\par\nointerlineskip\faDiamond\par\nointerlineskip\faDiamond}}
        \fi
    \fi
}
% -- enumerate
\setlist{itemsep=0pt, parsep=0pt, topsep=6pt}
%
\newenvironment{exercise}[1]{%
    \exersection
    \smallrrr
    \begin{enumerate}[%
        label=\thechapter.\arabic*,
        align=left,
        topsep=0pt,
        parsep=1pt,
        itemsep=6pt,
        labelsep=0.4em,
        labelwidth=2em,
        leftmargin=2.5em,
        listparindent=1em,
        font=\bfseries\sffamily]
        #1
    }{%
    \end{enumerate}
}
%
\newenvironment{exenum}[1]{%
    \begin{enumerate}[%
        label=(\arabic*), 
        leftmargin=2.25em, 
        labelsep=.75em, 
        parsep=0pt, 
        itemsep=0pt, 
        topsep=3pt]
        #1
    }{%
    \end{enumerate}
}
```

### 目录格式

目录共两级，格式用 `titletoc` 设置。

```latex
\RequirePackage{titletoc}
\renewcommand\contentsname{目\hspace{1em}次}
% ---- chapter
\titlecontents{chapter}[5.5em]
{\addvspace{10pt}\sffamily\bfseries\normalsize}
{\contentslabel{5.5em}\large}
{}
{}[\nointerlineskip]
% ---- section
%\dottedcontents{section}[5.3em]{}{3.2em}{3pt}
\titlecontents{section}[4.3em]
{\sffamily\small}
{ \mbox{\bfseries\contentslabel{3em}} }
{\hspace*{-2.6em}}
{\titlerule*[3pt]{.}\contentspage}
```

### 图、表和公式

图题、表题用 `caption` 宏包指定格式。

```latex
\RequirePackage { graphicx, booktabs }
\RequirePackage { caption } [ 2020/09/12 ]
\captionsetup[figure]{%
    name=図, 
    labelsep=quad,
    font={footnotesize,bf,sf}
}
\captionsetup[table]{%
    name=表,
    labelsep=quad,
    font={footnotesize,bf,sf}
}
```

公式使用左对齐，缩进约 2  全角字符。

```latex
\RequirePackage [ fleqn ]{ amsmath }
\setlength{\mathindent}{2em}
```

### 交叉引用

`hyperref` 和 `cleveref`。

```latex
\RequirePackage [ hidelinks ] { hyperref } 
\RequirePackage { cleveref }
\crefname{figure}{図}{図}
\crefname{table}{表}{表}
\crefname{equation}{式}{式}
\crefdefaultlabelformat{#2#1#3~}
```

### 封面

- 定制颜色。
  - 底纹浅绿 `c0`、深绿 `c1`。
  - 文字浅黄 `c2`。
  - 著者分隔点 `c3`。
- 棋盘纹理替换了类大理石纹理。
- 示例图片替换原图。

```latex
%
\definecolor{c0}{RGB}{84,171,198}
\definecolor{c1}{RGB}{74,150,173}
\definecolor{c2}{RGB}{255,250,205}
\definecolor{c3}{RGB}{95,162,162}
%
\begin{tikzpicture}[remember picture, overlay]
    \node [%
    anchor=north west,
    minimum width=0.676\paperwidth,
    minimum height=0.236\paperwidth] at ([xshift=0.053\paperwidth]current page.north west) (n1) {};
    
    \begin{scope}[on background layer]
    \path[draw,preaction={fill,c0},draw=none,pattern=checkerboard,pattern color=c1]
    decorate[decoration={zigzag,segment length=3.88mm, amplitude=1mm}]
    {(n1.north east)--(n1.south east)}--(n1.south west)|-cycle;
    \end{scope}
    
    \node [
    font=\normalsizerrr,
    align=left,draw=none,
    text=white,
    anchor=south west,
    inner ysep=1.5pt,
    inner xsep=2.25pt,
    scale=3.33
    ] at (n1.south west) (n2) 
    {
        {\sfbold 理工系}{\sflight のための} \\[-1.25ex] 
        {\sfbold 数学}{\sflight 入門}
    };
    
    \node [
    anchor=south west,
    align=left,text=c2,
    rotate=4,scale=1.1
    ] at ([xshift=3mm,yshift=1.0mm]n1.south) 
    {
        \xkcd Introduction to Mathematics \\[-1.2ex] 
        \xkcd for Science and Engineering \\[-1.2ex] 
        \xkcd learners
    };
    
    \node [
    anchor=north west,
    inner ysep=1.5pt,
    inner xsep=0pt,
    draw=white,scale=6.2,
    ] at (n1.south west) 
    (n3) 
    {
        {\bfseries\sfbold 確率}
        \kern-.25em{\sffamily・}\kern-.25em
        {\bfseries\sfbold 統計}
    };
    
    \node [
    anchor=north west,
    inner ysep=2.5pt,
    inner xsep=6.0pt,
    minimum height = 2em,
    draw=white,scale=1,
    ] 
    at (n3.south west) 
    (n4) 
    {
        \sflight\textbf{\large 菱田~博俊\,\textcolor{c3}{$\bullet$}}\,著
    };
    
    \fill [preaction={fill,c0}, pattern=checkerboard,pattern color=c1] ([xshift=-.99\paperwidth, yshift=0.5\paperheight]current page.south east)  arc(-180:0:.678\paperwidth) -- cycle;
    
    \node[anchor=north west,inner sep=0pt] 
    at ([xshift=2pt]current page.west) (img)
    {\includegraphics[width=.87\paperwidth, height=0.455\paperheight]{example-image-a}};
    
    \node[
    anchor=north west,
    inner xsep=2.5ex,
    inner ysep=2.0ex,
    scale=1.75,text=white] 
    at (img.north west)
    {
        \tamil Probability and Statistics
    };
    
    \node[anchor=south west,inner sep=0pt] 
    at ([xshift=10mm, yshift=-25mm]current page.center) 
    (img2)
    {
        \includegraphics[width=.38\paperwidth, height=0.32\paperheight]{example-image-b}
    };
    
    \node[anchor=south east] 
    at (img.south east)
    (logo) 
    {
        \includegraphics[scale=0.2]{example-image-c}
    };
\end{tikzpicture}
```

### 正文

- 字体替换。
- 图片替换。
- 其他基本还原。

### TODO

- 参考文献
- 索引