
### bubble sort

```
function bubble(list){
  let m = list.length;
  for(let i = 0; i < m; i++){
    for(let j = m - 1; j > i; j--){
      if(list[j] < list[j-1]){
        let temp = list[j];
        list[j] = list[j-1];
        list[j-1] = temp;
      }
    }
  }
  return list;
}



console.log(bubble([3,2,1,5,4,6,8,7]))

```