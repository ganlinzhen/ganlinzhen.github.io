---
layout: post
title: 前端算法
categories: JavaScript、算法
description: js 学习笔记
keywords: JavaScript, 算法
---
## 快速排序法两种

### 方法一、阮一峰的版本
注释：便于理解，但效率低些,用新数组来存放左右两边的内容
```javascript
var quickSort = function(arr) {
　　if (arr.length <= 1) { return arr; }
　　var pivotIndex = Math.floor(arr.length / 2);
　　var pivot = arr.splice(pivotIndex, 1)[0];
　　var left = [];
　　var right = [];
　　for (var i = 0; i < arr.length; i++){
　　　　if (arr[i] < pivot) {
　　　　　　left.push(arr[i]);
　　　　} else {
　　　　　　right.push(arr[i]);
　　　　}
　　}
　　return quickSort(left).concat([pivot], quickSort(right));
};
```
### 方法二、用指针来交换左右两边的内容
注释：较为高效
```javascript
// 交换的指针的函数
function swap(items, firstIndex, secondIndex){
    var temp = items[firstIndex];
    items[firstIndex] = items[secondIndex];
    items[secondIndex] = temp;
}
// 排序的主要过程，返回选中值的当前指针
function partition(items, left, right) {
    var pivot   = items[Math.floor((right + left) / 2)],
        i       = left,
        j       = right;
    while (i <= j) {
        while (items[i] < pivot) {
            i++;
        }
        while (items[j] > pivot) {
            j--;
        }
        if (i <= j) {
            swap(items, i, j);
            i++;
            j--;
        }
    }
    return i;
}
// 整体过程
function quickSort(items, left, right) {
    var index;
    if (items.length > 1) {
        index = partition(items, left, right);
        if (left < index - 1) {
            quickSort(items, left, index - 1);
        }
        if (index < right) {
            quickSort(items, index, right);
        }
    }
    return items;
}
```
## 冒泡排序法

```javascript
function bubbleSort (arr){
	for(var i=0;i<arr.length-1;i++){
		for(var j=i+1;j<arr.length;j++){
            //如果前面的数据比后面的大就交换
			if(arr[i]>arr[j]){
				var temp=arr[i];
				arr[i]=arr[j];
				arr[j]=temp;
			}
		}
	} 
	return arr;
}
```
## 二分查找法

注释：前提是数组是有序数组

```javascript
function binarySearch(target,arr,start,end) {
    var start   = start || 0;
    var end     = end || arr.length-1;

    var mid = parseInt(start+(end-start)/2);
    if(target==arr[mid]){
        return mid;
    }else if(target>arr[mid]){
        return binarySearch(target,arr,mid+1,end);
    }else{
        return binarySearch(target,arr,start,mid-1);
    }
    return -1;
}
```

// 二分法思想的应用---分治

```javascript
题目：在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

function Find(target, array) {
    let i = 0;
    let j = array[i].length - 1
    while (i < array.length && j >= 0) {
        if (array[i][j] < target) {
            i++;
            j = array[i].length - 1;
        } else if (array[i][j] > target) {
            j--;
        } else {
            return [i,j];
        }
    }
    return false;
}

//测试用例
console.log(Find(10, [
    [1, 2, 3, 4], 
    [5, 9, 10, 11], 
    [13, 20, 21, 23]
    ])
);
```
## 解析url中的参数

```javascript
function parseParam(url) {
    let obj = {};
    let arr = url.split("?");
    if (arr.length == 1) { //判断没有问号
        return "无参数"
    }
    if (single[0] == '') { //判断有？但是没有参数
        return '无参数'
    }
    let total = arr[1].split("&");
    for (let i = 0; i < total.length; i++) {
        let single = total[i].split("=");
        if (single[0] == '') { //判断有？但是没有参数
            return '无参数'
        }
        if (!single[1]) {
            obj[single[0]] = true;
        } else {
            if (obj[single[0]]) {
                let concat
                if (!Array.isArray(obj[single[0]])) { //判断是否数组
                concat = [obj[single[0]]]
                } else {
                concat = obj[single[0]];
                }
                concat.push(single[1]);
                concat = new Set(concat);
                concat = Array.from(concat) //数组去重
                obj[single[0]] = concat
            } else {
                obj[single[0]] = decodeURI(single[1]) //进行转码
            }
        }
    }
    return obj
}

var url = 'http://www.baidu.com/?user=huixin&id=123&id=456&city=%E5%8C%97%E4%BA%AC&enabled';

var params = parseParam(url)
```