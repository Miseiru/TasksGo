Задания 1-5 по производственной практике.
1. Написать одну из трёх сортировок статического массива (сортировку вставками, пузырьком, слиянием)
package main

import "fmt"

func insertionSort(arr []int) {
 n := len(arr)
 for i := 1; i < n; i++ {
  key := arr[i]
  j := i - 1
  for j >= 0 && arr[j] > key {
   arr[j+1] = arr[j]
   j--
  }
  arr[j+1] = key
 }
}

func main() {
 arr := []int{64, 34, 25, 12, 22, 11, 90}
 fmt.Println("Исходный массив:", arr)
 insertionSort(arr)
 fmt.Println("Отсортированный массив:", arr)
} 
________________________________________________ 

Написать программу которая хранит данные об офисных сотрудниках, сделать операцию добавления и удаления, максимум сколько можно добавить сотрудников 512 
package main

import (
 "fmt"
)

// Employee представляет данные о сотруднике офиса
type Employee struct {
 ID   int
 Name string
 Age  int
}

// Office представляет данные об офисе и его сотрудниках
type Office struct {
 employees []*Employee
 maxSize   int
}

// NewOffice создает новый офис с заданным максимальным размером
func NewOffice(maxSize int) *Office {
 return &Office{
  employees: make([]*Employee, 0),
  maxSize:   maxSize,
 }
}

// AddEmployee добавляет сотрудника в офис
func (o *Office) AddEmployee(employee *Employee) error {
 if len(o.employees) >= o.maxSize {
  return fmt.Errorf("офис заполнен, невозможно добавить сотрудника")
 }
 o.employees = append(o.employees, employee)
 return nil
}

// RemoveEmployee удаляет сотрудника из офиса по его ID
func (o *Office) RemoveEmployee(ID int) error {
 for i, emp := range o.employees {
  if emp.ID == ID {
   o.employees = append(o.employees[:i], o.employees[i+1:]...)
   return nil
  }
 }
 return fmt.Errorf("сотрудник с ID %d не найден", ID)
}

func main() {
 // Создаем новый офис с максимальным количеством сотрудников 512
 office := NewOffice(512)

 // Добавляем несколько сотрудников
 office.AddEmployee(&Employee{ID: 1, Name: "Иванов Иван", Age: 30})
 office.AddEmployee(&Employee{ID: 2, Name: "Петров Петр", Age: 35})
 office.AddEmployee(&Employee{ID: 3, Name: "Сидоров Сидор", Age: 40})

 // Выводим список сотрудников
 fmt.Println("Список сотрудников:")
 for _, emp := range office.employees {
  fmt.Printf("ID: %d, Имя: %s, Возраст: %d\n", emp.ID, emp.Name, emp.Age)
 }

 // Удаляем сотрудника по его ID
 err := office.RemoveEmployee(2)
 if err != nil {
  fmt.Println(err)
 }

 // Выводим обновленный список сотрудников
 fmt.Println("\nОбновленный список сотрудников:")
 for _, emp := range office.employees {
  fmt.Printf("ID: %d, Имя: %s, Возраст: %d\n", emp.ID, emp.Name, emp.Age)
 }
}
_________________________________________________________________________ 
2. Написать три структуры данных на выбор
   1) Стек
      package main

import "fmt"

type Stack struct {
 items []interface{}
}

func (s *Stack) Push(item interface{}) {
 s.items = append(s.items, item)
}

func (s *Stack) Pop() interface{} {
 if len(s.items) == 0 {
  return nil
 }
 index := len(s.items) - 1
 item := s.items[index]
 s.items = s.items[:index]
 return item
}

func main() {
 stack := Stack{}
 stack.Push(1)
 stack.Push(2)
 stack.Push(3)
 fmt.Println("Pop:", stack.Pop())   // Output: Pop: 3
 fmt.Println("Pop:", stack.Pop())   // Output: Pop: 2
}
___________________________________________________________________ 
2) Очередь
   package main

import "fmt"

type Queue struct {
 items []interface{}
}

func (q *Queue) Enqueue(item interface{}) {
 q.items = append(q.items, item)
}

func (q *Queue) Dequeue() interface{} {
 if len(q.items) == 0 {
  return nil
 }
 item := q.items[0]
 q.items = q.items[1:]
 return item
}

func main() {
 queue := Queue{}
 queue.Enqueue(1)
 queue.Enqueue(2)
 queue.Enqueue(3)
 fmt.Println("Dequeue:", queue.Dequeue()) // Output: Dequeue: 1
 fmt.Println("Dequeue:", queue.Dequeue()) // Output: Dequeue: 2
}
_________________________________________________________________
3) Бинарное дерево
   package main

import "fmt"

type Node struct {
 data  int
 left  *Node
 right *Node
}

func NewNode(data int) *Node {
 return &Node{data: data, left: nil, right: nil}
}

type BinaryTree struct {
 root *Node
}

func (bt *BinaryTree) Insert(data int) {
 if bt.root == nil {
  bt.root = NewNode(data)
  return
 }
 insertNode(bt.root, data)
}

func insertNode(node *Node, data int) {
 if data < node.data {
  if node.left == nil {
   node.left = NewNode(data)
  } else {
   insertNode(node.left, data)
  }
 } else {
  if node.right == nil {
   node.right = NewNode(data)
  } else {
   insertNode(node.right, data)
  }
 }
}

func main() {
 bt := BinaryTree{}
 bt.Insert(5)
 bt.Insert(3)
 bt.Insert(7)
 bt.Insert(1)
 bt.Insert(4)
 bt.Insert(6)
 bt.Insert(8)

 fmt.Println("Binary Tree:")
 printInOrder(bt.root)
}

func printInOrder(node *Node) {
 if node == nil {
  return
 }
 printInOrder(node.left)
 fmt.Println(node.data)
 printInOrder(node.right)
}
_______________________________________________________ 
Сделать конвертер из римских цифр в арабские 
package main

import (
 "fmt"
)

var romanNumerals = map[rune]int{
 'I': 1,
 'V': 5,
 'X': 10,
 'L': 50,
 'C': 100,
 'D': 500,
 'M': 1000,
}

func romanToArabic(roman string) int {
 total := 0
 prevValue := 0
 for _, r := range roman {
  value := romanNumerals[r]
  if value > prevValue {
   total += value - 2*prevValue
  } else {
   total += value
  }
  prevValue = value
 }
 return total
}

func main() {
 roman := "XLII"
 arabic := romanToArabic(roman)
 fmt.Printf("%s в арабских цифрах: %d\n", roman, arabic) // Output: XLII в арабских цифрах: 42
}
________________________________________________________________________________________ 
Заполнить двумерный массив случайными уникальными числами 
package main

import (
 "fmt"
 "math/rand"
 "time"
)

func generateUniqueRandomNumbers(size, max int) []int {
 rand.Seed(time.Now().UnixNano())
 uniqueNumbers := make(map[int]bool)
 numbers := make([]int, size)
 for i := 0; i < size; {
  num := rand.Intn(max)
  if !uniqueNumbers[num] {
   uniqueNumbers[num] = true
   numbers[i] = num
   i++
  }
 }
 return numbers
}

func main() {
 rows, cols := 3, 3
 maxValue := rows * cols
 matrix := make([][]int, rows)
 for i := range matrix {
  matrix[i] = generateUniqueRandomNumbers(cols, maxValue)
 }
 fmt.Println("Двумерный массив случайных уникальных чисел:")
 for _, row := range matrix {
  fmt.Println(row)
 }
}
________________________________________________________________________________ 
3. Переделать прошлое домашнее задание:

вынести структуры данных в отдельные пакеты
переделать функции которые обрабатывают структуры данных в методы
сделать структуры данных на дженериках  
1) Стек
   // stack.go

package stack

type Stack[T any] struct {
    items []T
}

func NewStack[T any]() *Stack[T] {
    return &Stack[T]{}
}

func (s *Stack[T]) Push(item T) {
    s.items = append(s.items, item)
}

func (s *Stack[T]) Pop() T {
    if len(s.items) == 0 {
        panic("stack is empty")
    }
    index := len(s.items) - 1
    item := s.items[index]
    s.items = s.items[:index]
    return item
}

func (s *Stack[T]) Peek() T {
    if len(s.items) == 0 {
        panic("stack is empty")
    }
    return s.items[len(s.items)-1]
} 
___________________________________________________________
2) Очередь 
// queue.go

package queue

type Queue[T any] struct {
    items []T
}

func NewQueue[T any]() *Queue[T] {
    return &Queue[T]{}
}

func (q *Queue[T]) Enqueue(item T) {
    q.items = append(q.items, item)
}

func (q *Queue[T]) Dequeue() T {
    if len(q.items) == 0 {
        panic("queue is empty")
    }
    item := q.items[0]
    q.items = q.items[1:]
    return item
}

func (q *Queue[T]) Peek() T {
    if len(q.items) == 0 {
        panic("queue is empty")
    }
    return q.items[0]
} 
________________________________________________________________
3) Бинарное дерево 
// binarytree.go

package binarytree

type Node[T any] struct {
    data  T
    left  *Node[T]
    right *Node[T]
}

func NewNode[T any](data T) *Node[T] {
    return &Node[T]{data: data, left: nil, right: nil}
}

type BinaryTree[T any] struct {
    root *Node[T]
}

func NewBinaryTree[T any]() *BinaryTree[T] {
    return &BinaryTree[T]{}
}

func (bt *BinaryTree[T]) Insert(data T) {
    if bt.root == nil {
        bt.root = NewNode(data)
        return
    }
    insertNode(bt.root, data)
}

func insertNode[T any](node *Node[T], data T) {
    switch {
    case data < node.data:
        if node.left == nil {
            node.left = NewNode(data)
        } else {
            insertNode(node.left, data)
        }
    default:
        if node.right == nil {
            node.right = NewNode(data)
        } else {
            insertNode(node.right, data)
        }
    }
}
______________________________________________________ 
4. Написать тесты на функции сортировки(сортировка пузырьком)
   // sorting_test.go

package main

import (
    "testing"
)

func TestBubbleSort(t *testing.T) {
    testCases := []struct {
        name     string
        input    []int
        expected []int
    }{
        {"Sorted array", []int{1, 2, 3, 4, 5}, []int{1, 2, 3, 4, 5}},
        {"Reverse sorted array", []int{5, 4, 3, 2, 1}, []int{1, 2, 3, 4, 5}},
        {"Random array", []int{3, 1, 4, 5, 2}, []int{1, 2, 3, 4, 5}},
        {"Array with duplicates", []int{3, 1, 4, 1, 5, 2, 4}, []int{1, 1, 2, 3, 4, 4, 5}},
    }

    for _, tc := range testCases {
        t.Run(tc.name, func(t *testing.T) {
            BubbleSort(tc.input)
            for i := range tc.input {
                if tc.input[i] != tc.expected[i] {
                    t.Errorf("Expected %v, got %v", tc.expected, tc.input)
                    break
                }
            }
        })
    }
} 
__________________________________________________________________
5. Написать тесты на операции со структурами данных
   1) Стек
      // stack_test.go

package stack

import (
    "testing"
)

func TestStack(t *testing.T) {
    s := NewStack[int]()
    s.Push(1)
    s.Push(2)
    s.Push(3)

    if s.Pop() != 3 {
        t.Error("Expected 3, got", s.Pop())
    }

    if s.Peek() != 2 {
        t.Error("Expected 2, got", s.Peek())
    }
} 
______________________________________________________
2) Очередь
   // queue_test.go

package queue

import (
    "testing"
)

func TestQueue(t *testing.T) {
    q := NewQueue[int]()
    q.Enqueue(1)
    q.Enqueue(2)
    q.Enqueue(3)

    if q.Dequeue() != 1 {
        t.Error("Expected 1, got", q.Dequeue())
    }

    if q.Peek() != 2 {
        t.Error("Expected 2, got", q.Peek())
    }
}
____________________________________________________ 
3) Бинарное дерево
   // binarytree_test.go

package binarytree

import (
    "testing"
)

func TestBinaryTree(t *testing.T) {
    bt := NewBinaryTree[int]()
    bt.Insert(5)
    bt.Insert(3)
    bt.Insert(7)
    bt.Insert(1)
    bt.Insert(4)
    bt.Insert(6)
    bt.Insert(8)

    testCases := []struct {
        name     string
        value    int
        expected bool
    }{
        {"Existing element", 3, true},
        {"Non-existing element", 10, false},
    }

    for _, tc := range testCases {
        t.Run(tc.name, func(t *testing.T) {
            if _, found := Search(bt.root, tc.value); found != tc.expected {
                t.Errorf("Expected %t, got %t", tc.expected, found)
            }
        })
    }
}
