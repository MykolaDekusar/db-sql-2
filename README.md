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
   SELECT
   `students`.`name`,
   `students`.`surname`,
   `degrees`.`name` AS `corso_di_laurea`
   FROM
   `students`
   INNER JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`
   WHERE
   `degrees`.`name` = "Corso di Laurea in Economia";
2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di
   Neuroscienze
   SELECT
   `degrees`.`name`,
   `departments`.`name` AS `dipartimento`
   FROM
   `degrees`
   INNER JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
   WHERE
   `degrees`.`name` LIKE 'Corso di Laurea Magistrale%'
   AND `departments`.`name` = 'Dipartimento di Neuroscienze';
3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
   SELECT
   `courses`.`name` AS `course_name`,
   `teachers`.`name`,
   `teachers`.`surname`
   FROM
   `courses`
   INNER JOIN `course_teacher` ON `courses`.`id` = `course_teacher`.`course_id`
   INNER JOIN `teachers` ON `course_teacher`.`teacher_id` = `teachers`.`id`
   WHERE
   `course_teacher`.`teacher_id` = 44

4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui
   sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e
   nome
   SELECT
   `students`.`name`,
   `students`.`surname`,
   `degrees`.`name` AS `degree`,
   `departments`.`name` AS `department`
   FROM
   `students`
   INNER JOIN `degrees` ON `students`.`degree_id` = `degrees`.`id`
   INNER JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
   ORDER BY
   `students`.`surname`,
   `students`.`name`;
5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti
   SELECT
   `degrees`.`name`,
   `teachers`.`name`,
   `teachers`.`surname`,
   `courses`.`name`
   FROM
   `course_teacher`
   INNER JOIN `teachers` ON `teachers`.`id` = `course_teacher`.`teacher_id`
   INNER JOIN `courses` ON `courses`.`id` = `course_teacher`.`course_id`
   INNER JOIN `degrees` ON `degrees`.`id` = `courses`.`degree_id`
6. Selezionare tutti i docenti che insegnano nel Dipartimento di
   Matematica (54)
   SELECT
   `teachers`.`id`,
   `teachers`.`name`,
   `teachers`.`surname`,
   `departments`.`name` AS `department_name`
   FROM
   `teachers`
   JOIN `course_teacher` ON `teachers`.`id` = `course_teacher`.`teacher_id`
   JOIN `courses` ON `course_teacher`.`course_id` = `courses`.`id`
   JOIN `degrees` ON `courses`.`degree_id` = `degrees`.`id`
   JOIN `departments` ON `degrees`.`department_id` = `departments`.`id`
   WHERE
   `departments`.`name` = 'Dipartimento di Matematica'
   GROUP BY
   `teachers`.`id`
   ORDER BY
   `teachers`.`id`;
7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti
   per ogni esame, stampando anche il voto massimo. Successivamente,
   filtrare i tentativi con voto minimo 18.
   SELECT
   `students`.`id` AS `student_id`,
   `students`.`name` AS `student_name`,
   `students`.`surname` AS `student_surname`,
   COUNT(`exam_student`.`vote`) AS `attempt_times`,
   MAX(`exam_student`.`vote`) AS `max_grade`,
   MIN(`exam_student`.`vote`) AS `min_grade`
   FROM
   `students`
   INNER JOIN `exam_student` ON `students`.`id` = `exam_student`.`student_id`
   INNER JOIN `exams` ON `exam_student`.`exam_id` = `exams`.`id`
   GROUP BY
   `students`.`id`,
   `exams`.`course_id`,
   `student_id`
   HAVING
   `max_grade` >= 18
   ORDER BY
   `student_name`,
   `student_surname`;
