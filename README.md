# gp_otus_hw4

-- тестирую read commited
-- 1 CЕССИЯ
create table transaction_test(id int, txt text)
insert into transaction_test values (1, 'f')

set transaction isolation level read committed;

begin
	set transaction isolation level read committed;

	insert into transaction_test values (2, 'b');
	select * from transaction_test;
	commit ;
end
-- 2 CЕССИЯ 
-- ДО ИСПОЛНЕНИЯ COMMIT ЗАПРОС ВЫДАВАЛ 1 ЗАПИСЬ
select * from transaction_test


-- тестирую ISOLATION LEVEL SERIALIZABLE
-- 1 сессия 
begin
	set transaction ISOLATION LEVEL SERIALIZABLE;
	SELECT * FROM transaction_test WHERE id = 1;
	UPDATE transaction_test SET txt = 'session1' WHERE id = 1;
	commit ;
end

-- 2 сессия 
begin
	set transaction ISOLATION LEVEL SERIALIZABLE;
	UPDATE transaction_test SET txt = 'session2' WHERE id = 1; -- Изменение данных
	commit ;
end
-- До исполнения commit в первой сессии запрос повис.
-- После commit в первой сессии получил ошибку
SQL Error [40001]: ERROR: could not serialize access due to concurrent update  (seg1 127.0.0.1:6001 pid=2281)

-- Вывод 
-- уровень изоляции SERIALIZABLE может привести к ошибкам в паралельных транзакциях.

-- Параметры хранения таблиц и загрузка внешних данных была произведенна в уроке2
https://github.com/DomashkinEduardMAF22/gp_otus_hm2/wiki/GP_OTUS_HW2
