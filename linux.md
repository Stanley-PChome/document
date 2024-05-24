## 檔案和目錄操作
```

顯示目錄內容
ls -l    #縮寫ll

切換至該目錄
cd /
cd ..    #上一層目錄

顯示當前目錄
pwd

建立目錄
mkdir directory
mkdir -p parent-directory/new-directory    #遞迴建立

刪除檔案或目錄
rm file
rm -r directory    # 遞迴刪除目錄
rm -f file         # 強制刪除

複製檔案或目錄
cp source_file destination
cp -r source_directory destination_directory   # 遞迴複製目錄

移動或重新命名檔案或目錄
mv old_name new_name
mv file /new/path


```

## 檔案內容處理
```

分頁顯示檔案內容
less file

持續輸出檔案最後幾行內容
tail -n 10 file

搜尋檔案或輸出內容
grep 'abc' file
                    grep -rnw '/var/www/html' -e '877' --include \*.php | grep -v 'vendor'

顯示系統Process(動態)
top

顯示當前Process信息(靜態)
ps
ps aux  # 顯示所有Process信息

顯示系統硬碟空間使用狀況
df
df -h  # 轉換空間單位

顯示目錄硬碟使用狀況
du
du -h  # 轉換空間單位

顯示系統記憶體使用狀況
free
free -h  # 轉換空間單位

``` 

## 檔案權限與擁有權
```

變更檔案權限
chmod 755 file

變更檔案擁有人
chown user:group file

```

## 網路相關指令
```

測試網路連接
ping example.com

使用ssh連至遠端主機
ssh user@hostname

```

## 安裝與管理指令 Ubuntu
```

sudo apt-get update  # 更新軟體
sudo apt-get upgrade  # 升級所有安裝軟體
sudo apt-get install package_name  # 安裝軟體
sudo apt-get remove package_name  # 刪除軟體

```

