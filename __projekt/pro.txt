-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema okulaj
-- -----------------------------------------------------

-- -----------------------------------------------------
-- Schema okulaj
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `okulaj` DEFAULT CHARACTER SET utf8 ;
USE `okulaj` ;

-- -----------------------------------------------------
-- Table `okulaj`.`projekt_Adres`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `okulaj`.`projekt_Adres` (
  `id_adresu` INT UNSIGNED NOT NULL AUTO_INCREMENT,
  `ulica` VARCHAR(45) NOT NULL,
  `adres` VARCHAR(45) NOT NULL,
  `kod` VARCHAR(6) NOT NULL,
  `miasto` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`id_adresu`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `okulaj`.`projekt_Uczniowie`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `okulaj`.`projekt_Uczniowie` (
  `id_ucznia` INT NOT NULL AUTO_INCREMENT,
  `Imie_ucznia` VARCHAR(45) NOT NULL,
  `nazwisko_ucznia` VARCHAR(45) NOT NULL,
  `PESEL` VARCHAR(11) NOT NULL,
  `nr_telefon` VARCHAR(12) NULL,
  `projekt_Adres_id_adresu` INT UNSIGNED NOT NULL,
  PRIMARY KEY (`id_ucznia`),
  INDEX `fk_projekt_Uczniowie_projekt_Adres1_idx` (`projekt_Adres_id_adresu` ASC) VISIBLE,
  CONSTRAINT `fk_projekt_Uczniowie_projekt_Adres1`
    FOREIGN KEY (`projekt_Adres_id_adresu`)
    REFERENCES `okulaj`.`projekt_Adres` (`id_adresu`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `okulaj`.`projekt_Poziom_nauczania`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `okulaj`.`projekt_Poziom_nauczania` (
  `id_poziomu` INT NOT NULL,
  `nazwa_poziomu` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`id_poziomu`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `okulaj`.`projekt_Jezyk`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `okulaj`.`projekt_Jezyk` (
  `id_jezyka` INT NOT NULL,
  `nazwa_jezyka` VARCHAR(45) NOT NULL,
  PRIMARY KEY (`id_jezyka`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `okulaj`.`projekt_Nauczyciele`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `okulaj`.`projekt_Nauczyciele` (
  `id_nauczyciela` INT NOT NULL AUTO_INCREMENT,
  `imie_nauczyciela` VARCHAR(45) NOT NULL,
  `nazwisko_nauczyciela` VARCHAR(45) NOT NULL,
  `PESEL` VARCHAR(11) NOT NULL,
  PRIMARY KEY (`id_nauczyciela`))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `okulaj`.`projekt_Placowka`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `okulaj`.`projekt_Placowka` (
  `id_placowki` INT NOT NULL,
  `projekt_Adres_id_adresu` INT UNSIGNED NOT NULL,
  PRIMARY KEY (`id_placowki`),
  INDEX `fk_projekt_Placowka_projekt_Adres1_idx` (`projekt_Adres_id_adresu` ASC) VISIBLE,
  CONSTRAINT `fk_projekt_Placowka_projekt_Adres1`
    FOREIGN KEY (`projekt_Adres_id_adresu`)
    REFERENCES `okulaj`.`projekt_Adres` (`id_adresu`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `okulaj`.`projekt_Kurs`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `okulaj`.`projekt_Kurs` (
  `id_kursu` INT NOT NULL,
  `data_poczatku` DATE NULL,
  `data_zakonczenia` DATE NULL,
  `projekt_Poziom_nauczania_id_poziomu` INT NOT NULL,
  `projekt_Jezyk_id_jezyka` INT NOT NULL,
  `projekt_Nauczyciele_id_nauczyciela` INT NOT NULL,
  `projekt_Placowka_id_placowki` INT NOT NULL,
  PRIMARY KEY (`id_kursu`),
  INDEX `fk_projekt_Kurs_projekt_Poziom_nauczania_idx` (`projekt_Poziom_nauczania_id_poziomu` ASC) VISIBLE,
  INDEX `fk_projekt_Kurs_projekt_Jezyk1_idx` (`projekt_Jezyk_id_jezyka` ASC) VISIBLE,
  INDEX `fk_projekt_Kurs_projekt_Nauczyciele1_idx` (`projekt_Nauczyciele_id_nauczyciela` ASC) VISIBLE,
  INDEX `fk_projekt_Kurs_projekt_Placowka1_idx` (`projekt_Placowka_id_placowki` ASC) VISIBLE,
  CONSTRAINT `fk_projekt_Kurs_projekt_Poziom_nauczania`
    FOREIGN KEY (`projekt_Poziom_nauczania_id_poziomu`)
    REFERENCES `okulaj`.`projekt_Poziom_nauczania` (`id_poziomu`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_projekt_Kurs_projekt_Jezyk1`
    FOREIGN KEY (`projekt_Jezyk_id_jezyka`)
    REFERENCES `okulaj`.`projekt_Jezyk` (`id_jezyka`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_projekt_Kurs_projekt_Nauczyciele1`
    FOREIGN KEY (`projekt_Nauczyciele_id_nauczyciela`)
    REFERENCES `okulaj`.`projekt_Nauczyciele` (`id_nauczyciela`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_projekt_Kurs_projekt_Placowka1`
    FOREIGN KEY (`projekt_Placowka_id_placowki`)
    REFERENCES `okulaj`.`projekt_Placowka` (`id_placowki`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `okulaj`.`projekt_Uczniowie_has_projekt_Kurs1`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `okulaj`.`projekt_Uczniowie_has_projekt_Kurs1` (
  `projekt_Uczniowie_id_ucznia` INT NOT NULL,
  `projekt_Kurs_id_kursu` INT NOT NULL,
  PRIMARY KEY (`projekt_Uczniowie_id_ucznia`, `projekt_Kurs_id_kursu`),
  INDEX `fk_projekt_Uczniowie_has_projekt_Kurs1_projekt_Kurs1_idx` (`projekt_Kurs_id_kursu` ASC) VISIBLE,
  INDEX `fk_projekt_Uczniowie_has_projekt_Kurs1_projekt_Uczniowie1_idx` (`projekt_Uczniowie_id_ucznia` ASC) VISIBLE,
  CONSTRAINT `fk_projekt_Uczniowie_has_projekt_Kurs1_projekt_Uczniowie1`
    FOREIGN KEY (`projekt_Uczniowie_id_ucznia`)
    REFERENCES `okulaj`.`projekt_Uczniowie` (`id_ucznia`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_projekt_Uczniowie_has_projekt_Kurs1_projekt_Kurs1`
    FOREIGN KEY (`projekt_Kurs_id_kursu`)
    REFERENCES `okulaj`.`projekt_Kurs` (`id_kursu`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `okulaj`.`projekt_Zaliczenie`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `okulaj`.`projekt_Zaliczenie` (
  `id_zaliczenia` INT NOT NULL AUTO_INCREMENT,
  `zaliczenie` VARCHAR(3) NOT NULL,
  `projekt_Uczniowie_has_projekt_Kurs1_projekt_Uczniowie_id_ucznia` INT NOT NULL,
  `projekt_Uczniowie_has_projekt_Kurs1_projekt_Kurs_id_kursu` INT NOT NULL,
  PRIMARY KEY (`id_zaliczenia`),
  INDEX `fk_projekt_Zaliczenie_projekt_Uczniowie_has_projekt_Kurs11_idx` (`projekt_Uczniowie_has_projekt_Kurs1_projekt_Uczniowie_id_ucznia` ASC, `projekt_Uczniowie_has_projekt_Kurs1_projekt_Kurs_id_kursu` ASC) VISIBLE,
  CONSTRAINT `fk_projekt_Zaliczenie_projekt_Uczniowie_has_projekt_Kurs11`
    FOREIGN KEY (`projekt_Uczniowie_has_projekt_Kurs1_projekt_Uczniowie_id_ucznia` , `projekt_Uczniowie_has_projekt_Kurs1_projekt_Kurs_id_kursu`)
    REFERENCES `okulaj`.`projekt_Uczniowie_has_projekt_Kurs1` (`projekt_Uczniowie_id_ucznia` , `projekt_Kurs_id_kursu`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB;


SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;
