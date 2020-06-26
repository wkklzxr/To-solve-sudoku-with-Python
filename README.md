# To-solve-sudoku-with-Python
## 使用Python解决数独问题
``` Python
"""
Python解决数独游戏
题目1
| |8|9|1| |3| | | |
| |2|7|4| | |8| | |
| | | |5|2| | | | |
| |7|6|9| | |5| |8|
|8| |3|6| |1|2| | |
| |4|5|3|8|7|1|6| |
|7|3|1|7|6|5|4| |2|
|9|3|1|7|6|5|4| |2|
|4| |2| | |9| | |6|

答案：
[5, 8, 9, 1, 7, 3, 6, 2, 4]
[3, 2, 7, 4, 9, 6, 8, 5, 1]
[6, 1, 4, 5, 2, 8, 9, 7, 3]
[1, 7, 6, 9, 4, 2, 5, 3, 8]
[8, 9, 3, 6, 5, 1, 2, 4, 7]
[2, 4, 5, 3, 8, 7, 1, 6, 9]
[7, 6, 8, 2, 1, 4, 3, 9, 5]
[9, 3, 1, 7, 6, 5, 4, 8, 2]
[4, 5, 2, 8, 3, 9, 7, 1, 6]

题目2
| |7|4| | | | |6| |
| | |9|2|3| | |7| |
| | | |5|2| | | | |
| |7|6|9| | |5| |8|
|8| |3|6| |1|2| | |
| |4|5|3|8|7|1|6| |
|7|3|1|7|6|5|4| |2|
|9|3|1|7|6|5|4| |2|
|4| |2| | |9| | |6|

[2, 7, 4, 5, 9, 8, 1, 6, 3]
[5, 1, 9, 2, 3, 6, 4, 7, 8]
[6, 3, 8, 7, 4, 1, 5, 9, 2]
[4, 9, 6, 1, 8, 3, 7, 2, 5]
[1, 2, 7, 4, 5, 9, 8, 3, 6]
[3, 8, 5, 6, 2, 7, 9, 4, 1]
[7, 4, 3, 8, 1, 2, 6, 5, 9]
[8, 6, 2, 9, 7, 5, 3, 1, 4]
[9, 5, 1, 3, 6, 4, 2, 8, 7]
"""

# author: WuKeke
# date: 2020-6-22

# 解决思路：
# 横、纵、九宫格求交集

# 代码部分
# 1. 初始化数独九宫格
import copy
import sys
sys.setrecursionlimit(1000000)

sudoku1 = [[] for i in range(9)]

sudoku1[0] = [' ', 8, 9, 1, ' ', 3, ' ', ' ', ' ']
sudoku1[1] = [' ', 2, 7, 4, ' ', ' ', 8, ' ', ' ']
sudoku1[2] = [' ', ' ', ' ', 5, 2, ' ', ' ', ' ', ' ']
sudoku1[3] = [' ', 7, 6, 9, ' ', ' ', 5, ' ', 8]
sudoku1[4] = [8, ' ', 3, 6, ' ', 1, 2, ' ', ' ']
sudoku1[5] = [' ', 4, 5, 3, 8, 7, 1, 6, ' ']
sudoku1[6] = [7, ' ', 8, 2, 1, 4, 3, 9, 5]
sudoku1[7] = [9, 3, 1, 7, 6, 5, 4, ' ', 2]
sudoku1[8] = [4, ' ', 2, ' ', ' ', 9, ' ', ' ', 6]


sudoku2 = [[] for i in range(9)]

sudoku2[0] = [' ', 7, 4, ' ', ' ', ' ', ' ', 6, ' ']
sudoku2[1] = [' ', ' ', 9, 2, 3, ' ', ' ', 7, ' ']
sudoku2[2] = [' ', ' ', ' ', ' ', 4, 1, 5, ' ', ' ']
sudoku2[3] = [' ', 9, 6, 1, ' ', ' ', ' ', ' ', ' ']
sudoku2[4] = [' ', ' ', ' ', ' ', ' ', 9, 8, 3, ' ']
sudoku2[5] = [' ', ' ', 5, 6, ' ', 7, 9, 4, ' ']
sudoku2[6] = [' ', ' ', 3, 8, ' ', 2, 6, 5, 9]
sudoku2[7] = [8, ' ', ' ', ' ', ' ', ' ', ' ', ' ', 4]
sudoku2[8] = [' ', ' ', 1, ' ', 6, ' ', 2, ' ', ' ']



# 对横轴各个格子求范围


def getPossibleHor(target):
    sudoku = copy.deepcopy(target)
    for i in range(9):
        horGrid = [k for k in range(1, 10)]
        for j in range(9):
            if sudoku[i][j] == ' ':
                continue
            horGrid.remove(sudoku[i][j])
        for j in range(9):
            if sudoku[i][j] == ' ':
                sudoku[i][j] = horGrid
    return sudoku


# 对纵轴各个格子求范围
def getPossibleVer(target):
    sudoku = copy.deepcopy(target)
    for i in range(9):
        horGrid = [k for k in range(1, 10)]
        for j in range(9):
            if sudoku[j][i] == ' ':
                continue
            horGrid.remove(sudoku[j][i])
        for j in range(9):
            if sudoku[j][i] == ' ':
                sudoku[j][i] = horGrid
    return sudoku


# 求九宫格范围
def getPossibleSudoku(target):
    sudoku = copy.deepcopy(target)
    for i in range(0, 9, 3):
        for j in range(0, 9, 3):
            horGrid = [k for k in range(1, 10)]
            for k in range(3):
                for l in range(3):
                    if sudoku[i+k][j+l] == ' ':
                        continue
                    horGrid.remove(sudoku[i+k][j+l])

            for k in range(3):
                for l in range(3):
                    if sudoku[i+k][j+l] == ' ':
                        sudoku[i+k][j+l] = horGrid

    return sudoku


# 求交集

def setupUnion(target):
    sudoku = copy.deepcopy(target)
    inithor = getPossibleHor(sudoku)
    initver = getPossibleVer(sudoku)
    initsodu = getPossibleSudoku(sudoku)

    for i in range(9):
        for j in range(9):
            if sudoku[i][j] == ' ':
                inithorset = set(inithor[i][j])
                initverset = set(initver[i][j])
                initsoduset = set(initsodu[i][j])
                sudoku[i][j] = list(
                    inithorset.intersection(initverset, initsoduset))

    return sudoku


def getFinalAnswer(target):
    sudoku = copy.deepcopy(target)
    unionTarget = setupUnion(sudoku)
    for i in range(9):
        for j in range(9):
            if not isinstance(unionTarget[i][j], int):
                if len(unionTarget[i][j]) == 1:
                    unionTarget[i][j] = unionTarget[i][j][0]
    for i in range(9):
        for j in range(9):
            if isinstance(unionTarget[i][j], list):
                unionTarget[i][j] = ' '

    for i in range(9):
        if ' ' in unionTarget[i]:
            return getFinalAnswer(unionTarget)
    return unionTarget


target = getFinalAnswer(sudoku1)

for i in target:
    print(i)
```

## 面向对象思维解决问题
``` Python
class SolveSudoku(object):
    """一个解决数独问题的程序"""

    def __init__(self, sudoku):
        self.sudoku = sudoku
        self.answer = None

    def getPossibleHor(self):
        """得到横轴各个格子的范围"""
        sudoku = copy.deepcopy(self.sudoku)
        for i in range(9):
            horGrid = [k for k in range(1, 10)]
            for j in range(9):
                if sudoku[i][j] == ' ':
                    continue
                horGrid.remove(sudoku[i][j])
            for j in range(9):
                if sudoku[i][j] == ' ':
                    sudoku[i][j] = horGrid
        return sudoku

    def getPossibleVer(self):
        """得到纵轴各个格子的范围"""
        sudoku = copy.deepcopy(self.sudoku)
        for i in range(9):
            horGrid = [k for k in range(1, 10)]
            for j in range(9):
                if sudoku[j][i] == ' ':
                    continue
                horGrid.remove(sudoku[j][i])
            for j in range(9):
                if sudoku[j][i] == ' ':
                    sudoku[j][i] = horGrid
        return sudoku

    def getPossibleSudoku(self):
        """得到3x3各个格子的范围"""
        sudoku = copy.deepcopy(self.sudoku)
        for i in range(0, 9, 3):
            for j in range(0, 9, 3):
                horGrid = [k for k in range(1, 10)]
                for k in range(3):
                    for l in range(3):
                        if sudoku[i+k][j+l] == ' ':
                            continue
                        horGrid.remove(sudoku[i+k][j+l])

                for k in range(3):
                    for l in range(3):
                        if sudoku[i+k][j+l] == ' ':
                            sudoku[i+k][j+l] = horGrid

        return sudoku

    def setupUnion(self, target):
        """求交集"""
        sudoku = copy.deepcopy(self.sudoku)
        inithor = self.getPossibleHor()
        initver = self.getPossibleVer()
        initsodu = self.getPossibleSudoku()

        for i in range(9):
            for j in range(9):
                if sudoku[i][j] == ' ':
                    inithorset = set(inithor[i][j])
                    initverset = set(initver[i][j])
                    initsoduset = set(initsodu[i][j])
                    sudoku[i][j] = list(
                        inithorset.intersection(initverset, initsoduset))
        self.sudoku = sudoku
        return self.sudoku

    def getFinalAnswer(self):
        """调用以上方法接口得到最后的答案"""
        sudoku = copy.deepcopy(self.sudoku)
        self.answer = self.setupUnion(sudoku)
        for i in range(9):
            for j in range(9):
                if not isinstance(self.answer[i][j], int):
                    if len(self.answer[i][j]) == 1:
                        self.answer[i][j] = self.answer[i][j][0]
        for i in range(9):
            for j in range(9):
                if isinstance(self.answer[i][j], list):
                    self.answer[i][j] = ' '

        for i in range(9):
            if ' ' in self.answer[i]:
                return self.getFinalAnswer()
        return self.answer
    
    def printAnswer(self):
        """对最终答案进行处理，得到一个标准的结果"""
        for eachAnswer in self.getFinalAnswer():
            print(eachAnswer)


solveSudoku = SolveSudoku(sudoku2)

solveSudoku.printAnswer()

``` 
