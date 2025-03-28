[[OS]]

>[!info] Definition
>**RAW-образ** - файл, который представляет собой точную копию данных, хранящихся на диске. Каждый байт в RAW-образе соответствует байту на физическом диске. 

## Характеристики RAW-образов
### Простота
RAW-образы не имеют сложной структуры или метаданных. Это делает их очень простыми в использовании и совместимыми с большинством инструментов.
### Размер
Размер RAW-образов фиксирован и равен размеру виртуального диска. Например, если создается RAW-образ на 10 ГБ, файл будет занимать 10 ГБ, даже если он пуст.
### Производительность
RAW-образы обеспечивают высокую производительность, так как не требуют дополнительной обработки данных (например, сжатия или управления блоками).
### Совместимость
RAW-образы совместимы с большинством инструментов виртуализации (QEMU, VirtualBox, VMware) и могут быть записаны на физические устройства (например, SD-карты или USB-накопители).
## Преимущества RAW-образов
### Высокая производительность
RAW-образы работают быстрее, чем форматы с дополнительными функциями (например, QCOW2), так как не требуют обработки данных.
### Универсальность
RAW-образы совместимы с большинством инструментов виртуализации и операционных систем.
### Простота использования
RAW-образы легко создавать, копировать и записывать на физические устройства.
### Поддержка физических устройств
RAW-образы можно напрямую записывать на SD-карты, USB-накопители или другие носители.
## Недостатки RAW-образов
### Большой размер
RAW-образы занимают много места на диске, даже если они не полностью заполнены данными.
### Отсутствие дополнительных функций
RAW-образы не поддерживают сжатие, шифрование, snapshots или другие функции, которые есть в более сложных форматах (например, QCOW2).
### Неэффективное использование места
Если образ пустой, он все равно будет занимать столько памяти на диске, сколько под него выделили.