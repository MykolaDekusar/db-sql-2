# Risolvere le query:

- Usando GROUP BY:

1. Contare quanti iscritti ci sono stati ogni anno
   SELECT
   YEAR(`enrolment_date`) AS 'year',
   COUNT(`enrolment_date`) AS 'total_registrations'
   FROM
   `students`
   GROUP BY
   YEAR(`enrolment_date`)
   ORDER BY
   YEAR(`enrolment_date`) ASC;
2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio
   SELECT
   `office_address` AS 'building',
   COUNT(\*) AS total_teachers
   FROM
   `teachers`
   GROUP BY
   `office_address`
   ORDER BY
   `office_address` ASC;
3. Calcolare la media dei voti di ogni appello d'esame
   SELECT
   `exam_id` AS esame,
   AVG(`vote`) AS average_vote
   FROM
   `exam_student`
   GROUP BY
   `exam_id`
   ORDER BY
   `exam_id` ASC;
4. Contare quanti corsi di laurea ci sono per ogni dipartimento
   SELECT
   `department_id` AS 'Department',
   COUNT(`address`) AS 'Total Courses'
   FROM
   `degrees`
   GROUP BY
   `department_id`
   ORDER BY
   `department_id`;

- Usando JOIN:

1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia
2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
   Neuroscienze
3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
   sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
   nome
5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
6. Selezionare tutti i docenti che insegnano nel Dipartimento di
   Matematica (54)
7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
   per ogni esame, stampando anche il voto massimo. Successivamente,
   filtrare i tentativi con voto minimo 18.
