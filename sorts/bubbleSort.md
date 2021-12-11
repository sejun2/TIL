```kotlin
import kotlin.random.Random

fun main(args: Array<String>) {
    try {
        val arrLength = doInputArrayLength()

        //Make random array with arrLength
        val unSortedArray = makeArrayWithLength(arrLength)

        //do bubble sort
        val myBubbleSort = MyBubbleSort()
        val sortedArray = myBubbleSort.sort(unSortedArray)

    } catch (e: Exception) {
        println(e)
    }
}

private fun makeArrayWithLength(arrLength: Int?): IntArray {
    val unSortedArray = IntArray(arrLength!!)
    for (i in 0 until arrLength!!) {
        unSortedArray[i] = Random.nextInt(999)
    }

    println("unSortedArray :: ${unSortedArray.contentToString()}")
    return unSortedArray
}

private fun doInputArrayLength(): Int? {
    print("[+]set length : ")
    val stdIn = readLine()?.toInt()

    return stdIn
}

class MyBubbleSort {
    fun swap(arr: IntArray, index1: Int, index2: Int) {
        val tmp = arr[index1]
        arr[index1] = arr[index2]
        arr[index2] = tmp
    }

    //오름차순 정렬
    fun sort(arr: IntArray): IntArray {
        for (i in 0 until arr.size) {
            for (j in 0 until arr.size -1) {
                if (arr[j] > arr[j + 1]) swap(arr, j, j + 1)
            }
        }

        println("sortedArray :: ${arr.contentToString()}")
        return arr
    }
}
```
