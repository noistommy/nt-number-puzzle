nt-number-puzzle
==================

`nt-number-puzzle` is a module that implements the retro game **numeric puzzle** as `vue` (vue components). Why don't you try to provide simple entertainment for users when developing a personal project or use it for users who are bored while loading long and long data?

`nt-number-puzzle`은 레트로 게임인 **숫자 퍼즐**을 `vue`(vue components)로 구현한 모듈입니다. 개인프로젝트 개발 시 사용자를 위한 간단한 즐길거리를 제공하려 하거나 길고 긴 데이터 로딩 중 심심한 사용자들을 위해 사용해 보는것은 어떨까요??

---
## Installation

```sh
 $ npm install nt-number-puzzle —-save
```
---
## Usage

```javascript
import NtNumberPuzzle from 'nt-number-puzzle';
import 'nt-number-puzzle/number-puzzle.css';

//template
//user`s source
...
<NtNumberPuzzle
  :grid-size="4" // default: 4
  :cell-size="4" // default: 4(rem)
  label-text = "..."
/>

```
---

## Props

* **gridSize**: _number_ ▶︎ `4`    
Setting for number tile grid`s vertical/horizontal size.

* **cellSize**: _number_ ▶︎ `4`   
Setting for tiles size(unit rem).

* **labelText**: _string_ ▶︎ `''`   
Setting for display tile text

---
