# Решения задач с LeetCode'a

package main

import "fmt"
```Golang
func main() {
	// maxSubArray([]int{-2, 1, -3, 4, -1, 2, 1, -5, 4})
	maxSubArray([]int{-2, 1, -3, 4, -1, 2, 1, -5, 4})
	maxArea([]int{2, 5, 4, 3, 6, 8, 6, 5, 3})
	maxProduct([]int{2, 3, -2, 4})
}

//! ============================================================================================================== 09.08.2023
const (
	UintSize = 32 << (^uint(0) >> 32 & 1)
	MaxInt   = 1<<(UintSize-1) - 1 // 1<<31 - 1 or 1<<63 - 1
	MinInt   = -MaxInt - 1
)

func maxSubArray(nums []int) int {

	currentMax := 0
	max := MinInt
	for i := 0; i < len(nums); i++ {
		currentMax *= nums[i]
		if currentMax > max {
			max = currentMax
		}

		if currentMax < 0 {
			currentMax = 0
		}

	}
	return max
}

//? ==================================================================================================================================

func min(a, b int) int { //*Функция для нахождения меньшего значения
	if a < b {
		return a
	}
	return b
}
func maxArea(height []int) int {
	res := 0                        //* Начальный показатель/результат
	left, right := 0, len(height)-1 //* Ставим начало и конец для поиска с двумя указателями
	for left < right {              //* Ставим while
		currentMax := min(height[left], height[right]) * (right - left) //*Функция min выдаёт нам меньшее число, которое мы умножаем на разницу между концом(right) и началом(left). Тут будет начальный показатель.
		if currentMax > res {                                           //*Условие для заноса ответа в res
			res = currentMax
		}
		if height[left] < height[right] { //*Услоиве для движения указателей
			left++
		} else {
			right--
		}
	}
	return res
}

func twoSum(nums []int, target int) []int {
	indexMap := make(map[int]int)  //Создаём мапу с длинной nums
	for currIndex, currNum := range nums { //Перебор каждого элемента массива, начиная с первого
		if requiredIdx, isPresent := indexMap[target-currNum]; isPresent { // В каждой итерации проверяем, присутствует ли требуемое число (target = nums - текуще ) в хэш-карте.
			return []int{requiredIdx, currIndex}  //Если присутствует, вернуть в качестве результата {требуемый числовой индекс, текущий числовой индекс} . 
		}
		indexMap[currNum] = currIndex
	}
	return []int{}
}

//?=======================================================================================

//Простой вариант
func longestCommonPrefix(strs []string) string {
    for i := 0 ;; i++  {
        for _, str := range strs {
            if i == len(str) || str[i] != strs[0][i] {
                return strs[0][:i]
            }
        }
    }
}

//?=======================================================================================

//Сложный вариант (затратный) 
func longestCommonPrefix(strs []string) string {
	lenStr := len(strs) 

	if lenStr == 0 { 
		return ""
	}

	firstString := strs[0]
	lenFirstString := len(firstString)

	commonPrefix := ""
	for i := 0; i < lenFirstString; i++ { 
		firstStringChar := string(firstString[i])
		match := true 
		for j := 1; j < lenStr; j++ { 
			if (len(strs[j]) - 1 < i) { 
				match = false 
				break
			}

			if string(strs[j][i]) != firstStringChar { 
				match = false
				break
			}	
		}
		if match { 
			commonPrefix += firstStringChar 
		} else { 
			break
		}
	}
	return commonPrefix 
}

//?=============================================================================================================================================

//Моё решение: 
func findMin(nums []int) int {
	sort.Ints(nums)
	return nums[0]
}


//==============================================================================================================================================
/* Другое решение 
1. Бинарный поиск 
2. Если nums[mid] > nums[high], минимальный в правую часть
3. Если nums[mid] < nums[high], минимальный в левую часть
4. Если nums[mid] == nums[high], минимальный в левую часть
5. Цикл закончится когда low == high
6. Возвращаем nums[low] */
func findMin(nums []int) int {
    n := len(nums)
    low, high := 0, n - 1
    for low < high {
        mid := (low + high) / 2
        if nums[mid] > nums[high] {
            low = mid + 1
        } else {
            high = mid
        }
    }
    return nums[low]
}


//?=============================================================================================================================================

//Поиск проводится с верхнего правого угла
func searchMatrix(matrix [][]int, target int) bool {
    rows, cols := len(matrix), len(matrix[0]) //Обозначем строки и столбцы
	r, c := 0, cols - 1 //Обозначем первые элементы строк и столбцов 
	for r < rows && c >= 0 {  //Пока первый элемент строки меньше длинны строки и первый эдемент столбца больше или равен 0
		if matrix[r][c] == target { //если нашли таргет, то возвращаем true 
			return true 
		}
		if target > matrix[r][c] { //если target нет в строке, то идём по строке дальше
			r++
		} else { //Если нет нет в столбце то спускаемся ниже 
			c--
		}
	}
	return false 
}

//Поиск с нижнего левого угла 
func searchMatrix(matrix [][]int, target int) bool { 
    rows, cols := len(matrix), len(matrix[0])
	r, c := rows - 1, 0

	for r >= 0 && c < cols {
		if matrix[r][c] == target {
			return true
		}
		if target > matrix[r][c] {
			c++
		} else { 
			r--
		}
	}
	return false 
}

//?=============================================================================================================================================

func productExceptSelf(nums []int) []int {
	n := len(nums)

	ans := make([]int, n)
	for i := range ans {
		ans[i] = 1
	}

	last := nums[0]
	fmt.Println(last)
	for i := 1; i < n; i++ {
		ans[i] *= last
		last *= nums[i]
		// fmt.Println(last)
	}
	last = nums[n-1]
	for i := n - 2; i >= 0; i-- {
		ans[i] *= last
		last *= nums[i]
	}

	return ans
}

//?============================================================================================================

func maxProduct(nums []int) int {
	res := nums[0]
	maxSum := math.MinInt

	for _, val := range nums { 
		currentMax = 
	}
}
```
