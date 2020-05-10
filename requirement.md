# Requirement

## About this project

This is a simplified real project. The purpose of this project is to assist Neuedu's sales staff in recruiting students from universities.

## Roles and responsibilities

There are users with four roles that will use this project: director of sales, channel sales, consultant, and administrator. Their responsibilities in this project are shown in the table below.

| Role              | Responsibility                                                                                                       |
| ----------------- | -------------------------------------------------------------------------------------------------------------------- |
| Director of Sales | Add and delete schools, and specify channel sales for each school. <br>Maintain college information for each school. |
| Channel sales     | Maintain information on admissions activities held at all schools he or she is responsible for.                      |
| Consultant        | Maintain the information of students obtained in each admissions activity.                                           |
| Administrator     | Maintain all users and roles. Assign roles to each user.                                                             |

## Modules

This project can be divided into 6 modules.

- **School**. Maintain schools and their colleges.
- **Activity**. Maintain admissions activities.
- **Student**. Maintain students.
- **User**. Maintain users.
- **Role**. Maintain roles.
- **Security**. User log in/out. Prevent users who are not logged in from accessing this project.

## Detailed requirements for each module

### 0. General requirements

- If the currently logged-in user does not have permission to access a module, then he or she should not see the menu to access the module.
- All APIs provided by the back-end (except login and logout) must start with `/api/`.
- All tables should be paginated. If the total number of data in the table will definitely not exceed 100 (such as roles), then you can use front-end paging (the back-end sends all data to the front-end at one time, and the front-end controls paging), otherwise you must use back-end paging (the front-end sends page number and page size to the back-end, the back-end only returns data that meets the paging parameters).
- There should be a search bar above each table, and users can query the data through some main fields (such as school name, student name, role name, etc.).
- After adding, modifying, or deleting a certain data, a corresponding prompt should be given according to whether the operation was successful (for example, save successfully, delete failed, etc.). See [Message](https://element.eleme.cn/#/en-US/component/message).
- Before deleting a certain data, the user should be prompted whether to delete the data. See [Message Box](https://element.eleme.cn/#/en-US/component/message-box#confirm), [Popconfirm](https://element.eleme.cn/#/en-US/component/popconfirm).
- The following fields are not sent from the front-end: creatorId, creatorName, createdTime. The creatorId is the id of current user. The creatorName is the name of current user. The createdTime should be `now()` when you insert a record to database.

### 1. School module

- Only users with the `director of sales` role can access this module.
- The school model should contain the following fields: id, name, channelSalesId, channelSalesName, colleges, creatorId, creatorName, createdTime.
- The school table shown in the front-end should have the following columns: School Name(`name`), Channel Sales(`channelSalesName`), Creator(`creatorName`), Created Time(`createdTime`) and Operations(college button, edit button, delete button, etc.).
- When adding / modifying a school, the user must specify a `channel sales`(a user with this role) for the school.
- One school can have multiple colleges.
- The College model should contain the following fields: id, name, schoolId.
- If no activities are held in a school, then this school can be deleted. Otherwise it is not allowed to delete this school.
- When deleting a school, all colleges in that school should also be deleted.
- If a college is not associated with any student, then the college can be deleted. Otherwise, this college is not allowed to be deleted.

### 2. Activity module

- Only users with the `channel sales` role can access this module.
- The activity model should contain the following fields: id, name, form, schoolId, schoolName, startDate, endDate, teacherName, consultantId, consultantName, remarks, creatorId, creatorName, createdTime.
- The activity table shown in the front-end should have the following columns: Activity Name(`name`), School Name(`schoolName`), Start Date(`startDate`), End Date(`end date`), Consultant Name(`consultantName`), Remarks(`remarks`), Created Time(`createdTime`) and Operations(edit button, delete button, etc.).
- In the activity table shown in the front-end, the user can only see activities he or she created.
- When adding / modifying an activity, the user can only choose a school from the schools he or she is responsible for.
- When adding / modifying an activity, the user must specify a `consultant`(a user with this role) for the activity.
- The form of activity can be selected from the following: admissions (value is 1), experience class (value is 2) and others (value is 0).
- If the form of the activity is an experience class, there will be an additional input box to enter the name of the teacher(`teacherName`). Otherwise, this input box should be invisible.
- If an activity is not associated with any student, then the activity can be deleted. Otherwise, this activity is not allowed to be deleted.

### 3. Student module

- Only users with the `consultant` role can access this module.
- The student model should contain the following fields: id, activityId, name, phoneNumber, schoolName, collegeId, collegeName, remarks, creatorId, creatorName, createdTime.
- The student table shown in the front-end should have the following columns: Student Name(`name`), Phone Number(`phoneNumber`), School Name(`schoolName`), College Name(`collegeName`), Remarks(`remarks`), Created Time(`createdTime`) and Operations(edit button, delete button, etc.).
- In the student table shown in the front-end, the user can only see students he or she created.
- When adding / modifying a student, the user can only choose an activity from the activities he or she is responsible for.
- After selecting the activity, the user must choose a college from the school.

### 4. User module

- Only users with the `administrator` role can access this module.
- The user model should contain the following fields: id, username, password, name, roles, status(0 for disabled, 1 for enabled), creatorId, creatorName, createdTime.
- The user table shown in the front-end should have the following columns: Username(`username`), Name(`name`), Roles, Status(`status`), Creator(`creatorName`), Created Time(`createdTime`) and Operations(role button, edit button, delete button, etc.).
- Username field must be unique.
- Password is not allowd to be stored in plain text. It should be encrypted by MD5, SHA-1 or **BCrypt**(recommended).
- A user can have multiple roles.
- A default user with role `administrator` must be initialized.

### 5. Role module

- Only users with the `administrator` role can access this module.
- The role model should contain the following fields: id, name.
- The role table shown in the front-end should have the following columns: Role Name(`name`), and Operations(user button, edit button, delete button, etc.).
- Name field must be unique.
- The four roles: `Director of Sales`, `Channel sales`, `Consultant`, `Administrator` must be initialized.

### 6. Security module

- All back-end APIs except login are only allowed to be called after login, otherwise a 403 status code is returned.
- Only accounts with user status enabled can log in.
- This module must provide a method to obtain information about the currently logged in user.
- This module must provide a API for the front-end to obtain information about the currently logged in user.
- We use `Spring Security` int this project. You can get some infomation from [Spring Boot Reference](https://docs.spring.io/spring-boot/docs/2.2.7.RELEASE/reference/html/spring-boot-features.html#boot-features-security), [Spring Security Reference](https://docs.spring.io/spring-security/site/docs/5.2.4.RELEASE/reference/htmlsingle/).

## Extra features

If you want more work, you can try to do the following.

### Validations

In a real project, all data entered by the user must validated before being saved in the database. The validation can be done in front-end (see [Validation](https://element.eleme.cn/#/en-US/component/form#validation)), back-end (see [Java Bean Validation](https://docs.spring.io/spring/docs/5.2.6.RELEASE/spring-framework-reference/core.html#validation-beanvalidation), [ Hibernate Validator](https://docs.jboss.org/hibernate/stable/validator/reference/en-US/html_single/)), or both (recommended).

You can decide the verification rules by yourself, these rules must be reasonable.
