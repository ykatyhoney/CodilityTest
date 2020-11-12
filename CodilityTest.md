# Task 1
### Tasks
- When click the button, increase the counter number
- Set the default counter number as 42
- Set `h2` class as `counter`
```
<template>
  <div>
    <h2 class="counter">{{counter}}</h2>
    <button class="counter-button" v-on:click="increase()">Click</button>
  </div>
</template>

<script>
export default {
  name: 'counter',
  data: ()=>({
      counter: 42
  }),
  methods: {
      increase(){
          this.counter++;
      }
  }
};
</script>

<style>
.counter {
    
}
.counter-button {
  font-size: 1rem;
  padding: 5px 10px;
  color: #585858;
}
</style>
```

# Task 2
### Tasks
- Get N brands list the user likes.
`U` is `{id: 123, gender: 'FEMALE'}`, `N` is amount which the user likes.

`getLikedBrands(U.id)`: get the brands the user likes. The result is like `[{122, 'Nike'}, {2212, 'MBL}]`.

`getTopBrandsForGender(U.gender)`: get the brands a gender likes. The result is like `[{122, 'Nike'}, {2212, 'MBL}]`.

If the result amount is not smaller than N,  get the brands that gender likes.

If brand amount the user likes and the brand amount the gender likes are totally smaller than `N`, return CustomerError with `"Not enough data"` message.
```
'use strict';

function solution(U, N) {
    return new Promise((resolve, reject) => {
        if(N<1 || !U){
            reject(new CustomError('Not enough data'));
        }
        let result = [];
        getLikedBrands(U.id).then(data=>{
            let likedBrands = data.map(d=>d.name);
            if(likedBrands.length<N){
                getTopBrandsForGender(U.gender).then(data1=>{
                    let topBrands = data1.map(d=>d.name)
                    // Remove the duplications at likedBrands and TopBrands.
                    result = [...new Set([...likedBrands, ...topBrands])];
                    if(result.length < N){
                        reject(new CustomError('Not enough data'));
                    }else{
                        resolve(result.slice(0, N));
                    }
                }).catch(()=>{
                    reject(new CustomError('Not enough data'));
                })
            }else{
                result = likedBrands.slice(0, N);
                resolve(result);
            }
        }).catch(()=>{
            reject(new CustomError('Not enough data'));
        })
    });
}
```