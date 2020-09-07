从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。

**示例 1:**

```
输入: [1,2,3,4,5]
输出: True
```



**示例 2:**

```
输入: [0,0,1,2,5]
输出: True
```



**限制：**

数组长度为 5 

数组的数取值为 [0, 13] .



#### 思路1

分别计算可填补牌的0的个数，以及空缺牌的个数，并最后进行比较

中途需要注意代码顺序



```java
class Solution {
    public boolean isStraight(int[] nums) {
        Arrays.sort(nums);
        int count = 0;
        int diff = 0;
        for(int i = 0; i < nums.length - 1; i++){
            if(nums[i] == 0){
                count++;
                continue;
            }
            if(nums[i] == nums[i + 1]){
                return false;
            }
            if(nums[i] + 1 != nums[i + 1]){
                diff += nums[i + 1] - nums[i] - 1;
            }
        }
        
        return count >= diff;
    }
}
```







#### 思路2

先计算任意牌0的数目，排序后从非0开始依次遍历，遇到差的牌去检查0是否还够用

```java
class Solution {
    public boolean isStraight(int[] nums) {
        Arrays.sort(nums);
        int count = 0;
        int index = 0;
        for(int num : nums){
            if(num == 0) count++;
        }
        index = count;
        for(int i = index; i < nums.length - 1; i++){
            if(nums[i] == nums[i + 1]) return false;
            if(nums[i] + 1 == nums[i + 1]) continue;
            count -= nums[i + 1] - 1 - nums[i];
            if(count < 0) return false;
        }
        return true;
    }
}
```



#### 思路3

更新max和min以此判断最后max-min是否小于5

```java
class Solution {
    public boolean isStraight(int[] nums) {
        int max = 0, min = 13;
        Set<Integer> set = new HashSet<>();
        for(int num : nums){
            if(num == 0) continue;
            max = Math.max(num, max);
            min = Math.min(num, min);
            if(set.contains(num)) return false;
            set.add(num);
        }
        return max - min < 5;
    }
}
```

