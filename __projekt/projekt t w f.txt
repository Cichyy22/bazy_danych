DELIMITER $$
CREATE TRIGGER Data_poczatku_konca
BEFORE INSERT ON okulaj.projekt_Kurs
FOR EACH ROW 
BEGIN
set NEW.data_poczatku = "2021-09-01";
END; $$

DELIMITER $$
CREATE TRIGGER Data_konca
BEFORE INSERT ON okulaj.projekt_Kurs
FOR EACH ROW 
BEGIN
set NEW.data_zakonczenia = date_add("2021-09-01", interval 6 month);
END; $$

DELIMITER //
CREATE TRIGGER Bledny_kod
BEFORE INSERT ON okulaj.projekt_Adres
FOR EACH ROW
BEGIN
  IF NEW.kod not like '__-___'
  THEN
    SET NEW.kod = 'blad';
  END IF;
END
//
DELIMITER ;


DELIMITER $$
CREATE PROCEDURE wyrzucenie(IN id int)
BEGIN
delete from  okulaj.projekt_Uczniowie  where id_ucznia = id;
END
$$
DELIMITER ;

DELIMITER $$
CREATE PROCEDURE kto_zaliczyl (IN id int, out osoby varchar(255))
BEGIN
select concat(projekt_Uczniowie_has_projekt_Kurs1_projekt_Uczniowie_id_ucznia,' ', zaliczenie) into osoby  
from okulaj.projekt_Zaliczenie
where projekt_Uczniowie_has_projekt_Kurs1_projekt_Uczniowie_id_ucznia=id;
END
$$
DELIMITER ;

DELIMITER //
CREATE FUNCTION ile_uczniow()
RETURNS int
BEGIN
DECLARE ilu varchar(100);
SELECT count(id_ucznia) into ilu from okulaj.projekt_Uczniowie;
RETURN ilu;
END //

DELIMITER //
CREATE FUNCTION ile_niezdal()
RETURNS int
BEGIN
DECLARE ile int;
SELECT count(zaliczenie)
into ile from okulaj.projekt_Zaliczenie 
where zaliczenie='nie';
RETURN ile;
END //

DELIMITER //
CREATE FUNCTION kiedy_poczatek(id int)
RETURNS date
BEGIN
DECLARE kiedy date;
SELECT data_poczatku
into kiedy from okulaj.projekt_Kurs where id_kursu=id ;
RETURN kiedy;
END //
