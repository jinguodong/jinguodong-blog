---
layout: post
title: iscroll-5学习笔记
categories:
- 前段开发
tags:
- HTML
- CSS
- JS
---

## HTML内所有元素的默认display形态

    .dcss html,.dcss address,.dcss blockquote,.dcss body,.dcss dd,.dcss div,
    .dcss dl,.dcss dt,.dcss fieldset,.dcss form,.dcss frame,.dcss frameset,
    .dcss h1,.dcss h2,.dcss h3,.dcss h4,.dcss h5,.dcss h6,.dcss noframes,
    .dcss ol,.dcss p,.dcss ul,.dcss center,.dcss dir,.dcss hr,.dcss menu, 
    .dcss pre { display: block }
    .dcss li { display: list-item }
    .dcss head { display: none }
    .dcss table { display: table }
    .dcss tr { display: table-row }
    .dcss thead { display: table-header-group }
    .dcss tbody { display: table-row-group }
    .dcss tfoot { display: table-footer-group }
    .dcss col { display: table-column }
    .dcss colgroup { display: table-column-group }
    .dcss td,.dcss  th { display: table-cell; }
    .dcss caption { display: table-caption }
    .dcss th { font-weight: bolder; text-align: center }
    .dcss caption { text-align: center }
    .dcss body { margin: 8px; line-height: 1.12 }
    .dcss h1 { font-size: 2em; margin: .67em 0;line-height: 1.5em }
    .dcss h2 { font-size: 1.5em; margin: .75em 0;line-height: 1.5em }
    .dcss h3 { font-size: 1.17em; margin: .83em 0;line-height: 1.5em }
    .dcss h4 { font-size: 1.09em; margin: 1.12em 0;line-height: 1.5em }
    .dcss h4,.dcss p,.dcss blockquote,.dcss ul,.dcss fieldset, .dcss form,.dcss ol,.dcss dl,.dcss dir,.dcss menu { margin: 1.12em 0 }
    .dcss h5 { font-size: .83em; margin: 1.5em 0;line-height: 1.5em }
    .dcss h6 { font-size: .75em; margin: 1.67em 0;line-height: 1.5em }
    .dcss h1, .dcss h2, .dcss h3, .dcss h4, .dcss h5, .dcss h6, .dcss b,.dcss strong { font-weight: bolder }
    .dcss blockquote { margin-left: 40px; margin-right: 40px }
    .dcss i, .dcss cite, .dcss em,.dcss var,.dcss address { font-style: italic }
    .dcss pre, .dcss tt,.dcss  code,.dcss  kbd,.dcss samp { font-family: monospace }
    .dcss pre { white-space: pre }
    .dcss button,.dcss textarea,.dcss input,.dcss object,.dcss select { display:inline-block; }
    .dcss big { font-size: 1.17em }
    .dcss small, .dcss sub, .dcss sup { font-size: .83em }
    .dcss sub { vertical-align: sub }
    .dcss sup { vertical-align: super }
    .dcss table { border-spacing: 2px; }
    .dcss thead, .dcss tbody,.dcss tfoot { vertical-align: middle }
    .dcss td, .dcss th { vertical-align: inherit }
    .dcss s, .dcss strike,.dcss  del { text-decoration: line-through }
    .dcss hr { border: 1px inset }
    .dcss ol, .dcss ul,.dcss  dir, .dcss menu, .dcss dd { margin-left: 40px }
    .dcss ol { list-style-type: decimal }
    .dcss ol ul,.dcss ul ol, .dcss ul ul, .dcss ol ol { margin-top: 0; margin-bottom: 0 }
    .dcss u,.dcss ins { text-decoration: underline }
    .dcss br:before { content: "A" }
    .dcss :before, .dcss :after { white-space: pre-line }
    .dcss center { text-align: center }
    .dcss abbr,.dcss acronym { font-variant: small-caps; letter-spacing: 0.1em }
    .dcss :link,.dcss :visited { text-decoration: underline }
    .dcss :focus { outline: thin dotted invert }
    .dcss BDO[DIR="ltr"] { direction: ltr; unicode-bidi: bidi-override }
    .dcss BDO[DIR="rtl"] { direction: rtl; unicode-bidi: bidi-override }
    .dcss *[DIR="ltr"] { direction: ltr; unicode-bidi: embed }
    .dcss *[DIR="rtl"] { direction: rtl; unicode-bidi: embed }
    @media print {
        .dcss h1 { page-break-before: always }
        .dcss h1,.dcss h2,.dcss h3,.dcss h4,.dcss h5,.dcss h6 { page-break-after: avoid }
        .dcss ul,.dcss ol,.dcss dl { page-break-before: avoid }
    }

## jQuery操作DOM函数一览

