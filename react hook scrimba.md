## 1. Intro
- What we will learn:
1. useState
2. useEffect
3. useRef
4. useCallback
5. useMemo
6. React.memo
7. Custom hooks
## 2. useState() Part 1
- learn array destructuring
- useState will return a array, first element is the value of the state and the second element is a function to update the state
```js
import React, { useState } from 'react'
import randomColor from 'randomcolor'

export default function Playground() {
  // ini state-state nya
  // buat ngubah statenya, setternya ditaruh di fungsi biar gampang, trs dalam setter bikin fungsi lagi buat ngatur perubahan statenya karena state itu asyncronus
  // kenapa harus ada currrentCount kenapa nggk langsung count tanpa bikin fungsi lagi? karena state itu asyncrous dan lebih aman ada currentStatenya biar tw yg diupdate itu state yang sekarang dipake
  const [count, setCount] = useState(0)
  return (
    <div>
      {count}
      // ingat ! kalo ada eventListener dalemnya harus ada () =>, jika nggk manggil fungsi, karena nerima event
      <button onClick={() => setCount(currentCount => currentCount - 1)}>-</button>
      <button onClick={() => setCount(currentCount => currentCount + 1)}>+</button>
    </div>
  )
}

// https://cdb.reacttraining.com/react-inline-functions-and-performance-bdff784f5578
``` 

# Begin to make Painting App
## 3. useState() Part 2
- make a project name
  ```js
  import React, { useState } from 'react'

  export default function Name() {
    const [name, setName] = useState('')
    return (
      <label className="header-name">
        <input
          value={name}
          onChange={e => setName(e.target.value)}
          onClick={e => e.target.setSelectionRange(0, e.target.value.length)}
          placeholder="Untitled"
        />
      </label>
    )
  }
  ```
# 4. useEffect()



