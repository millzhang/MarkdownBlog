### 文件弹框        

```c
QString fileName = QFileDialog::getOpenFileName(this, "Open the file");
QFile file(fileName);
```

### 提示框

```c
QMessageBox::warning(this,"..","No file opened. Use Save As");
```

### 中文显示编码

在`main`中添加

```c
QTextCodec::setCodecForLocale (QTextCodec::codecForName ("UTF8"));
```

### `qt`打包

> http://blog.csdn.net/windsnow1/article/details/78004265


### 退出程序

```c
void Notepad::on_actionExit_triggered()
{
    if (!(QMessageBox::information(this,tr("温馨提示"),tr("是否确定退出程序?"),tr("嗯，是的！"),tr("点错了"))))
    {
        this->close();
    }
}
```

#### 退出监听

```c
void closeEvent(QCloseEvent *event);

void Notepad::closeEvent(QCloseEvent *event)
{
    if (!(QMessageBox::information(this,tr("温馨提示"),tr("是否确定退出程序?"),tr("嗯，是的！"),tr("点错了"))))
    {
        this->close();
    }
}

```

### 设置文字颜色

```
ui->textEdit->setTextColor(QColor(14,234,25,255));
```