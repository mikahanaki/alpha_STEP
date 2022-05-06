# alpha_STEP
make png and csv in output folder

# -*- coding: utf-8 -*-
"""
Created on Wed May  4 09:31:32 2022

@author: mikam
"""
#数値処理
import numpy as np

#データの可視化
from matplotlib import pyplot as plt

#"output"ﾌｫﾙﾀﾞを作成
import os
dirname = "output/"
os.makedirs(dirname, exist_ok=True) #既にﾌｫﾙﾀﾞが存在していてもERRORでない

#日本語表示
plt.rcParams['font.family'] = 'Meiryo'

#textﾃﾞｰﾀから値を読み込む（ファイル名、スキップする行、列単位でひとくくりにする）
#段差計ﾃﾞｰﾀはutf-8なのでそのまま使える
dansa01_axis1, dansa_value1= np.loadtxt("dansa.txt",skiprows=0,unpack=True)

#グラフ表示エリアの範囲設定（X方向,Y方向）単位はインチ
fig = plt.figure(figsize=(10, 4))

#グラフを何個表示して、何個目にプロットするか
ax = fig.add_subplot(111)

#グラフのマーカー、色、ラベルの設定
ax.plot(dansa01_axis1, dansa_value1, "-", color="k", label="段差ﾃﾞｰﾀ")


#軸のラベル設定
ax.set_xlabel("走査距離/mm")
ax.set_ylabel("高さ/mm")

#凡例の表示位置
ax.legend(loc="upper left")

#グリッドを表示しています。
plt.grid()

#グラフを表示
plt.show()



# グラフをimg.pngという名前の画像ファイルとして保存
#fig.savefig("img.png")
filename = dirname + "img.png"
fig.savefig(filename)

#描画範囲を取得する
xmin, xmax = ax.get_xlim()
ymin, ymax = ax.get_ylim()
print(f"x:[{xmin},{xmax}],y:[{ymin},{ymax}]")


#テキストデータをcsvに変換して出力する
import pandas as pd
read_text_file = pd.read_csv (r"dansa.txt")
read_text_file.to_csv (r"dansa.csv",  index=None)


#csvファイルのﾃﾞｰﾀをタブで区切って"dansa_out.csv"として出力
import csv
with open('dansa.csv', 'r', newline='') as filein, \
        open('output\dansa_out.csv', 'w', newline='') as fileout:
 
        reader = csv.reader(filein, delimiter='\t', skipinitialspace=True)
        writer = csv.writer(fileout)
 
        writer.writerows(reader)
        

#不要になった”dansa.csv"ファイルの削除
os.remove('dansa.csv')

print('file_out.csvへの出力完了')


