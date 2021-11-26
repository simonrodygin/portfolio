Для каждой отдельной книги необходимо вывести информацию о количестве проданных экземпляров и их стоимости за текущий и предыдущий год.
Вычисляемые столбцы назвать Количество и Сумма. Информацию отсортировать по убыванию стоимости.

Схема БД по ссылкам: https://ucarecdn.com/d5c5d16e-32eb-4caa-8fc8-6982cda2ac32/
					 https://ucarecdn.com/bd089cc7-acb0-442e-a276-4995f7ade4b1/
select title, sum(Кол) as Количество, sum(Сум) as Сумма
from
    (select title, buy_archive.amount as Кол, buy_archive.amount*buy_archive.price as Сум
    from buy_archive
    join book using (book_id)
   union all
    select title, buy_book.amount as Кол, buy_book.amount*price as Сум
    from book
    join buy_book using (book_id)
    join buy using (buy_id)
    join buy_step using (buy_id)
    where step_id=1 and buy_step.date_step_end is not null) as prom
group by title
order by Сумма desc