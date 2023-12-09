class Student:
  def __init__(self, name, surname, gender):
      self.name = name
      self.surname = surname
      self.gender = gender
      self.finished_courses = []
      self.courses_in_progress = []
      self.grades = {}
      self.evaluates_lectures = []  
  def rate_lecturer(self, lecturer, course, grade):
      if isinstance(lecturer, Lecturer) and course in self.evaluates_lectures and course in lecturer.teaches_the_course:
          if course in lecturer.grades:
              lecturer.grades[course] += [grade]
          else:
              lecturer.grades[course] = [grade]
      else:
          return 'Ошибка'
  
  def av_gr(self):
    sum_gr = 0
    len_gr = 0
    for course in self.grades.values():
        sum_gr += sum(course)
        len_gr += len(course)

    average_grade = round(sum_gr / len_gr, 1) if len_gr > 0 else 0
    return float(average_grade)  
  def __lt__(self, other):
    return self.av_gr() < other.av_gr()
  
  def set_evaluates_lectures(self, courses):
    self.evaluates_lectures = courses
  
  def __str__(self):
    return f"Имя: {self.name} \nФамилия: {self.surname}\nСредняя оценка за домашние задания: {self.av_gr()} \nКурсы в процессе изучения: {', ' .join(self.courses_in_progress)} \nЗавершенные курсы: {', ' .join(self.finished_courses)}"



class Mentor:
  def __init__(self, name, surname):
      self.name = name
      self.surname = surname
      self.courses_attached = []



class Lecturer(Mentor):
  def __init__(self, name, surname):  
      super().__init__(name, surname)
      self.teaches_the_course = []
      self.grades = {} 
    
  def av_gr(self):
        sum_gr = 0
        len_gr = 0
        for course in self.grades.values():
          sum_gr += sum(course)
          len_gr += len(course)

        average_grade = round(sum_gr / len_gr, 1) if len_gr > 0 else 0
        return float(average_grade)
      
  def __lt__(self, other):
      return self.av_gr() < other.av_gr()
      
  def __str__(self):
      return f"Имя: {self.name} \nФамилия: {self.surname} \nСредняя оценка за лекции: {self.av_gr()}"
      
      

class Rewiewer(Mentor):
        def rate_hw(self, student, course, grade):
          if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
              if course in student.grades:
                  student.grades[course] += [grade]
              else:
                  student.grades[course] = [grade]
          else:
              return 'Ошибка'
          
          
        
        def __str__(self):
          return f"Имя: {self.name} \nФамилия: {self.surname}"


       
def average_grade_students(students, course):
    all_grades = []
    for student in students:
        if course in student.grades:
          all_grades += student.grades[course]
          
        else:
                  return 'No Data'  
        if all_grades:
            result = sum(all_grades) / len(all_grades)
            return result
        else:
            return 0

def average_grade_lectors(lectors, course):
  all_grades = []
  for lector in lectors:
      if course in lector.grades:
          all_grades += lector.grades[course]
      else:
          return 'No Data'  
  if all_grades:  
      result = sum(all_grades) / len(all_grades)
      return result
  else:
      return 0
    



student = Student('Ilya', 'Grigoryev', 'male')
student.courses_in_progress += ['Python']
student.set_evaluates_lectures(['Python'])
student.finished_courses = ['Введение в программирование']

student_1 = Student('Elena', 'Archipova', 'famale')
student_1.courses_in_progress += ['Контекстная реклама']
student_1.set_evaluates_lectures(['Контекстная реклама'])
student_1.finished_courses = ['Основы маркетинга']

student_2 = Student('Azalia', 'Yusupova', 'famale')
student_2.courses_in_progress += ['Дизайн']
student_2.set_evaluates_lectures(['Дизайн'])
student_2.finished_courses = ['Работа в илюстраторе']

mentor_r = Rewiewer('Victor', 'Ivanov')
mentor_r.courses_attached += ['Python']

mentor_re = Rewiewer('Konstantin', 'Pavlov')
mentor_re.courses_attached += ['Контекстная реклама']

mentor_rew = Rewiewer('Galina', 'Nikolaeva')
mentor_rew.courses_attached += ['Дизайн']

mentor_l = Lecturer('Vladimir', 'Smirnov')
mentor_l.teaches_the_course += ['Python']

mentor_le = Lecturer('Ivan', 'Vasilyev')
mentor_le.teaches_the_course += ['Контекстная реклама']

mentor_lec = Lecturer('Grigory', 'Vasilevskiy')
mentor_lec.teaches_the_course += ['Дизайн']

mentor_r.rate_hw(student, 'Python', 10)
mentor_r.rate_hw(student, 'Python', 9)
mentor_r.rate_hw(student, 'Python', 8)

mentor_re.rate_hw(student_1, 'Контекстная реклама', 10)
mentor_re.rate_hw(student_1, 'Контекстная реклама', 8)
mentor_re.rate_hw(student_1, 'Контекстная реклама', 8)

mentor_rew.rate_hw(student_2, 'Дизайн', 10)
mentor_rew.rate_hw(student_2, 'Дизайн', 7)
mentor_rew.rate_hw(student_2, 'Дизайн', 9)

student.rate_lecturer(mentor_l, 'Python', 10)
student.rate_lecturer(mentor_l, 'Python', 9)
student.rate_lecturer(mentor_l, 'Python', 9)

student_1.rate_lecturer(mentor_le, 'Контекстная реклама', 9)
student_1.rate_lecturer(mentor_le, 'Контекстная реклама', 8)
student_1.rate_lecturer(mentor_le, 'Контекстная реклама', 9)

student_2.rate_lecturer(mentor_lec, 'Дизайн', 10)
student_2.rate_lecturer(mentor_lec, 'Дизайн', 10)
student_2.rate_lecturer(mentor_lec, 'Дизайн', 8)

students = []
students.append(student)
students.append(student_1)
students.append(student_2)

lectors = []
lectors.append(mentor_l)
lectors.append(mentor_le)
lectors.append(mentor_lec)

lecturer_average_python = average_grade_lectors(lectors, 'Python')
lecturer_average_context_ad = average_grade_lectors(lectors, 'Контекстная реклама')
lecturer_average_design = average_grade_lectors(lectors, 'Дизайн')

student_average_python = average_grade_students(students, 'Python')
student_average_python = average_grade_students(students, 'Контекстная реклама')
student_average_python = average_grade_students(students, 'Дизайн')

if lecturer_average_python and student_average_python and lecturer_average_python != 'No Data' and student_average_python != 'No Data':
  if lecturer_average_python > student_average_python:
      print("Средняя оценка преподавателя выше, чем средняя оценка студента")
  else:
      print("Средняя оценка студента выше чем средняя оценка преподавателя")
else:
  print("Средние оценки недоступны для сравнения")

print(student)
print(student_1)
print(student_2)

print(mentor_r)
print(mentor_re)
print(mentor_rew)

print(mentor_l)
print(mentor_le)
print(mentor_lec)

print(f'{mentor_l.av_gr()}, {student.av_gr()}')
print(f'{mentor_le.av_gr()}, {student_1.av_gr()}')
print(f'{mentor_lec.av_gr()}, {student_2.av_gr()}')


print(f'{mentor_le.av_gr()}, {student_1.av_gr()}')
print(mentor_le > student_1)
print(mentor_le < student_1)
print()
print(student_2.grades)
print()
print(f'Средняя оченка за {student_1.courses_in_progress} = {student_1.av_gr()} балла')
print()