```#!/bin/bash

# Папка, из которой нужно удалить все файлы
TARGET_DIR="/path/to/directory"

# Проверка, существует ли директория
if [ ! -d "$TARGET_DIR" ]; then
  echo "Указанная директория не существует: $TARGET_DIR"
  exit 1
fi

# Удаление всех файлов в директории
rm -f "$TARGET_DIR"/*

# Удаление всех файлов в поддиректориях
find "$TARGET_DIR" -type f -exec rm -f {} \;

echo "Все файлы в директории $TARGET_DIR были удалены."
```
