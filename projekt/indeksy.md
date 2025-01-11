## Indeksy na kluczach obcych


### Tabela class_attendance
```sql
-- dbo.class_attendance
CREATE INDEX idx_class_attendance_class_id ON dbo.class_attendance(class_id);
CREATE INDEX idx_class_attendance_student_id ON dbo.class_attendance(student_id);
```
### Tabela course_details
```sql
-- dbo.course_details
CREATE INDEX idx_course_details_course_id ON dbo.course_details(course_id);
```
### Tabela course_enrollment
```sql
-- dbo.course_enrollment
CREATE INDEX idx_course_enrollment_course_id ON dbo.course_enrollment(course_id);
CREATE INDEX idx_course_enrollment_user_id ON dbo.course_enrollment(user_id);
```
### Tabela course_modules
```sql
-- dbo.course_modules
CREATE INDEX idx_course_modules_course_id ON dbo.course_modules(course_id);
CREATE INDEX idx_course_modules_module_id ON dbo.course_modules(module_id);
```
### Tabela courses
```sql
-- dbo.courses
CREATE INDEX idx_courses_product_id ON dbo.courses(product_id);
```
### Tabela diplomas
```sql
-- dbo.diplomas
CREATE INDEX idx_diplomas_course_id ON dbo.diplomas(course_id);
CREATE INDEX idx_diplomas_student_id ON dbo.diplomas(student_id);
```
### Tabela final_exams
```sql
-- dbo.final_exams
CREATE INDEX idx_final_exams_study_id ON dbo.final_exams(study_id);
CREATE INDEX idx_final_exams_student_id ON dbo.final_exams(student_id);
```
### Tabela internship_attendance
```sql
-- dbo.internship_attendance
CREATE INDEX idx_internship_attendance_study_id ON dbo.internship_attendance(study_id);
CREATE INDEX idx_internship_attendance_student_id ON dbo.internship_attendance(student_id);
```
### Tabela invoices
```sql
-- dbo.invoices
CREATE INDEX idx_invoices_user_id ON dbo.invoices(user_id);
```
### Tabela lecture_details
```sql
-- dbo.lecture_details
CREATE INDEX idx_lecture_details_lecturer_id ON dbo.lecture_details(lecturer_id);
CREATE INDEX idx_lecture_details_lecture_id ON dbo.lecture_details(lecture_id);
```
### Tabela module_attendance
```sql
-- dbo.module_attendance
CREATE INDEX idx_module_attendance_course_module_id ON dbo.module_attendance(course_module_id);
CREATE INDEX idx_module_attendance_student_id ON dbo.module_attendance(student_id);
```
### Tabela module_online_meeting
```sql
-- dbo.module_online_meeting
CREATE INDEX idx_module_online_meeting_course_module_id ON dbo.module_online_meeting(course_module_id);
```
### Tabela open_lectures
```sql
-- dbo.open_lectures
CREATE INDEX idx_open_lectures_class_id ON dbo.open_lectures(class_id);
```
### Tabela payments
```sql
-- dbo.payments
CREATE INDEX idx_payments_invoice_id ON dbo.payments(invoice_id);
CREATE INDEX idx_payments_user_id ON dbo.payments(user_id);
```
### Tabela shopping_carts
```sql
-- dbo.shopping_carts
CREATE INDEX idx_shopping_carts_user_id ON dbo.shopping_carts(user_id);
```
### Tabela shopping_carts_products
```sql
-- dbo.shopping_carts_products
CREATE INDEX idx_shopping_carts_products_product_id ON dbo.shopping_carts_products(product_id);
CREATE INDEX idx_shopping_carts_products_cart_id ON dbo.shopping_carts_products(cart_id);
```
### Tabela studies
```sql
-- dbo.studies
CREATE INDEX idx_studies_product_id ON dbo.studies(product_id);
```
### Tabela study_enrollment
```sql
-- dbo.study_enrollment
CREATE INDEX idx_study_enrollment_study_id ON dbo.study_enrollment(study_id);
CREATE INDEX idx_study_enrollment_user_id ON dbo.study_enrollment(user_id);
```
### Tabela study_subject_attendance
```sql
-- dbo.study_subject_attendance
CREATE INDEX idx_study_subject_attendance_class_id ON dbo.study_subject_attendance(class_id);
CREATE INDEX idx_study_subject_attendance_user_id ON dbo.study_subject_attendance(user_id);
```
### Tabela study_subject_classes
```sql
-- dbo.study_subject_classes
CREATE INDEX idx_study_subject_classes_study_subject_id ON dbo.study_subject_classes(study_subject_id);
```
### Tabela study_subject
```sql
-- dbo.study_subjects
CREATE INDEX idx_study_subjects_study_id ON dbo.study_subjects(study_id);
CREATE INDEX idx_study_subjects_subject_id ON dbo.study_subjects(subject_id);
```
### Tabela user_address
```sql
-- dbo.user_address
CREATE INDEX idx_user_address_user_id ON dbo.user_address(user_id);
```
### Tabela users
```sql
-- dbo.users
CREATE INDEX idx_users_role_id ON dbo.users(role_id);
```
### Tabela webinars
```sql
-- dbo.webinars
CREATE INDEX idx_webinars_webinar_id ON dbo.webinars(webinar_id);
CREATE INDEX idx_webinars_product_id ON dbo.webinars(product_id);
CREATE INDEX idx_webinars_recording_id ON dbo.webinars(recording_id);
```
