## Задание
Исходные данные: 
Даны функции максимальной суточной производительности по двум подземным хранилищам газа (ПХГ). Каждое ПХГ заполнено на 90% (файл 1.Кривые_ПХГ.xlsx). 
Имеется прогноз будущей потребности в ресурсе (отборах) из всех ПХГ в совокупности (2.Прогноз_ПХГ.csv).
Задание: 
На период прогноза требуется найти оптимальное распределение отборов газа из каждого ПХГ такое, чтобы
- Суммарная производительность по всем ПХГ была бы максимальной на 01.12.2021.
- Совокупные суточные отборы полностью покрывали бы потребность.
- Суточные отборы по каждому ПХГ не превышали бы максимальную производительность.

## Решение

### Анализ исходных данных
1) Загрузим данные о необходимых прогнозных потребностях отбора газа
2) На основе известных функций зависимости максимальной суточной производительности для каждого ПХГ, рассчитаем текущие значения (при заполненности на 90%), а также построим кривые максимальной производительности при каждом значении от 0 до текущего уровня.

### Определение задачи оптимизации
3) Для решения задачи оптимизации воспользуемся модулем нелинейного программирования из библиотеки SciPy Minimize. Он дает возможность минимизировать целевую функцию, накладывая также некоторые необходимые ограничения.
  В качестве метода для функции minimize будем использовать SLSQP, как один из наиболее эффективных алгоритмов, использующийся для последовательной оптимизации задач.
Определим вектор прогнозных потребностей и создадим нулевой вектор в качестве исходных значений. Главное, чтоб по длине он был в 2 раза больше вектора потребностей, чтобы сохранять результаты моделирования для двух ПХГ.
  -Определим целевую функцию. В ней на вход подается вектор с ежедневными отборами, первые и последние 30 символов разделяются на 2 переменные, описывающие ежедневные отборы из каждого хранилища. Далее из исходной заполненности ПХГ кумулятивно вычитаются все ежедневные отборы для нахождения конечного остатка в последний день прогноза, и на основе этого остатка считается максимальная производительность для каждого ПХГ. Сумма этих производительностей и является целевым значением, которое мы хотим максимизировать.
  -Далее необходимо определить функции ограничения. В функции constraint1 на вход подается тот же вектор с прогнозными отборами, разделяются по двум переменным. Далее создается еще один нулевой вектор, который затем ежедневно на основе текущего отбора и остатка в каждом ПХГ заполняется максимально возможной суточной производительностью. Значения в нашем итоговом векторе не должны превышать этих значений.
  -Также определим функцию, которая рассчитывает разницу между суммарными уровнями отбора и прогнозными потребностями (constraint2). Эта разница должна быть равно 0 для полного покрытия потребностей.

### Формирование результатов
4) По результатам моделирования мы получаем оптимальные распределения отборов газа по каждому ПХГ, а также значение суммарной максимальной производительности на последний день прогноза при таких отборах (≈20 млн куб.м).
5) Занесем в таблицу рассчитанные ежедневные оптимальные отборы и остатки для каждого ПХГ.
6) Нанесем на ранее построенный график уровень оптимального отбора и оставшейся заполненности для каждого ПХГ. Можем убедиться, что ни в один из дней не будет отобрано больше, чем максимально возможно.

### Анализ результатов
7) Также построим график, на котором изобразим прогнозную потребность отбора, полученные оптимальные отборы по каждому ПХГ, а также суммарный оптимальный уровень отбора. Можем убедиться, что результат не меньше прогнозных значений, но максимально  к нему близок, что говорит о корректном решении задачи минимизации.
8) Также можем построить столбчатую диаграмму для каждого ПХГ, описывающую уровень отбора и уровень оставшихся объемов.
