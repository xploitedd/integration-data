[![ionproject.org](https://raw.githubusercontent.com/i-on-project/integration/master/img/i-on_logo.png)](https://www.ionproject.org)

# Introduction
The I-On initiative aims to build an extensible platform that is the reference point to the academic community, aggregating published information regarding term calendars, timetables, courses, and other items of relevance.

I-On Integration is a sub-project of the initiative and its main goal is to collect unstructured data from external sources and make the information available in a format that can be understood by other components.

This repository holds the strutured data made available by I-On Integration making it available to other I-On sub-projects.

# Data Structure

The following diagram depics the structure defined to maintain the data.

![Data Structure](https://github.com/i-on-project/integration-data/blob/master/img/I-On_Integration-Data_Structure.png)

The name of the folders for the schools will be based on the package naming prefix used by Java using the reversed Internet domain name for each institution qualified name.

Each school will have a folder `academic_years`with the several academic years containing that year's academic calendar in the format `YYYY-YYYY`. The institution will also have a folder named `programmes` containing the several programmes breaked down by calendar term (semester) in the format `YYYY-YYYY-[1|2]` where the last digit refers either to the first (`1`) or the second semester (`2`).

The academic data is splitted in several files that separate the multiple entities:
  - [Schools](#schools)
  - [Courses](#courses)
  - [School Programmes](#school-programmes)
  - [Timetables](#timetables)
  - [Exam Schedule](#exam-schedule)
  - [Academic Calendar](#academic-calendar)

## Schools
Provides details of schools. The primary key is the `id` field which uses same naming prefix as described above for the folders.

```yml
schools:
  - id: pt.ipl.isel
    countryCode: pt
    name: Instituto Superior de Engenharia de Lisboa  
    address: Rua Conselheiro Emídio Navarro, 1, 1959-007 LISBOA
    telephone: (+351) 218317000
    uri: https://www.isel.pt
    email: isel@isel.pt
  - id: pt.ulisboa.tecnico
    countryCode: pt
    name: Instituto Superior Técnico
    address: Av. Rovisco Pais, 1049-001 LISBOA
    telephone: (+351) 218417769
    uri: https://www.tecnico.ulisboa.pt
    email: academica@tecnico.ulisboa.pt
```

## Courses
Describes all the course offering for a school. The courses have an `id` which provides uniquivocal reference. The `acronym` is an array since there may be multiple acronyms associated with the course (e.g. Segurança de Informação is known as SI and SegInf).

```yml
school: pt.ipl.pt
courses:
  - id: 0
    acronym: 
      - PSC
    name: Programação em Sistemas Computacionais
    termNumber: 
      - 3
    optional: false
    termDuration: 1 # 1 .. 4
    ects: 6
    scientificArea: IC
    programmes:
      - LEIC
  - id: 1
    acronym:
      - CD
    name: Computação Distribuída
    termNumber:
      - 1
    optional: false
    termDuration: 1 # 1 .. 4
    ects: 6
    scientificArea: IC
    programmes:
      - MEIC
```

## School Programmes
This format describes the programmes provided by an institution. States the list of teachers that coordinate it, the department, number of terms, email, uri to an external reference and its description. The `acronym` provides the primary key.

```yml
school: pt.ipl.pt
programmes:
  - acronym: MEIC
    name: Mestrado em Engenharia Informática e de Computadores
    termSize: 6
    department: ADEETC
    coordination:
      - Teacher A
      - Teacher B
      - Teacher C
    email: meic.coordenacao@deetc.isel.ipl.pt
    uri: https://www.isel.pt/cursos/mestrados/engenharia-informatica-e-de-computadores
    description: O mestrado em Engenharia Informática e de Computadores (MEIC), corresponde ao 2º ciclo no âmbito da Declaração de Bolonha. O MEIC oferece aos seus estudantes uma formação sólida com o ADN do ISEL - equilíbrio entre conceitos teóricos e a sua aplicação prática - recorrendo às mais modernas tecnologias. A organização curricular do MEIC permite ao estudante construir um percurso de formação individualizado, direcionado para quem pretende aprofundar as competências adquiridas no ciclo de estudos anterior, mas também para atualizar os conhecimentos e melhorar as competências de quem já está no mercado de trabalho.
  - acronym: LEIC
    termSize: 6
    department: ADEETC
    coordination:
      - Teacher B
      - Teacher D
    email: leic.coordenacao@deetc.isel.ipl.pt
    uri: https://www.isel.pt/cursos/licenciaturas/engenharia-informatica-e-de-computadores
    description: O ciclo de estudos conducente ao grau de licenciado em Engenharia Informática e de Computadores resulta do aprofundamento dum perfil profissional reconhecido pelo mercado empregador nacional e internacional. A inovação e o aperfeiçoamento pedagógico reconhecidos são consequência da vasta experiência do corpo docente e do seu envolvimento em atividades de investigação, desenvolvimento e de projeto, em colaboração com empresas e outras entidades, públicas e privadas. Este ciclo de estudos reflete a influência da ligação à indústria, da interação com a comunidade. Trata-se duma experiência continuada de relacionamento produtivo, acompanhada pela transferência de tecnologia, formação e consultadoria, em diferentes áreas de especialização da engenharia informática e de computadores.
```

## Timetables

The `timetable.yml` file will be formatted as follows: 

```yml
creationDateTime: 2021-04-21T20:49:16Z      # Coordinated Universal Time (UTC) - ISO 8601
retrievalDateTime: 2021-04-21T20:49:16Z      # Coordinated Universal Time (UTC) - ISO 8601
school:
  name: "INSTITUTO SUPERIOR DE ENGENHARIA DE LISBOA"
  acr: ISEL
programme:
  name: "Licenciatura em Engenharia Informática e de Computadores"
  acr: LEIC
calendarTerm: 2020-2021-2
classes:
  -   acr: E
      sections:
        - section: LEIC11Da
          events:
            - category: LECTURE  # [LECTURE | PRACTICE | LAB | LECTURE_PRACTICE]
              location:
                - L_H2    # not mandatory
              beginTime: "14:00"
              duration: "01:30"
              weekday: MO # RFC 5545  [ SU | MO | TU | WE | TH | FR | SA ]
            - category: LECTURE
              beginTime: "14:00"
              duration: "01:30"
              weekday: WE
            - category: LECTURE
              beginTime: "14:00"
              duration: "01:30"
              weekday: TH
          instructors:
            - name: Teacher A
              category: PRACTICE
            - name: Teacher B
              category: LECTURE
```

## Exam Schedule

For the `exam_schedule.yaml` file will have the format:

```yml
creationDateTime: 2021-04-21T20:49:16Z
retrievalDateTime: 2021-04-21T20:49:16Z
school:
    name: Instituto Superior Engenharia Lisboa 
    acr: ISEL
programme:
    name: Licenciatura em Engenharia Informática e de Computadores
    acr: LEIC
calendarTerm: 2019-2020-2
exams:
    - name: AED
      startDate: 2020-07-09T14:00:00.000
      endDate: 2020-07-09T17:00:00.000
      category: TEST #TEST | EXAM_NORMAL | EXAM_ALTERN | EXAM_SPECIAL
      location: 
```

The formats of each key are similar to the ones used in `timetable.yml`, with the exception of `category`. This key will classify the evaluations with **one** the following values `[ TEST | EXAM_NORMAL | EXAM_ALTERN | EXAM_SPECIAL ]`.

## Academic Calendar

For the `calendar.yml`:

```yml
creationDateTime: 2021-04-21T20:49:16Z
retrievalDateTime: 2021-04-21T20:49:16Z
school:
  name: Instituto Superior Engenharia Lisboa 
  acr: ISEL
language: pt-PT
terms:
  - calendarTerm: 2019-2020-1
    interruptions:
      - name: Férias Natal
        startDate: 2019-12-23
        endDate: 2020-01-04
    evaluations:
      - name: Exames época normal
        startDate: 2020-01-13
        endDate: 2020-02-01
        duringLectures: false
      - name: Exames época recurso
        startDate: 2020-02-03
        endDate: 2020-02-15
        duringLectures: false
      - name: Exames época especial
        startDate: 2020-02-26
        endDate: 2020-03-07
        duringLectures: true
    lectures:
      - name: Turmas 1º Semestre
        curricularTerm:
          - id: 1
        startDate: 2019-09-16
        endDate: 2020-01-11
      - name: Turmas excepto 1º Semestre
        curricularTerm:
          - id: 2
          - id: 3
          - id: 4
          - id: 5
          - id: 6
        startDate: 2019-09-09
        endDate: 2019-12-21
    otherEvents:
      - name: Divulgação de horários
        startDate: 2019-07-22
        endDate: 2019-07-22
      - name: Abertura das atividades letivas 2019/2020
        startDate: 2019-09-02
        endDate: 2019-09-02
      - name: Data limite para lançamento de classificações no Portal Académico (frequência, exames de época normal e de época de recurso)
        startDate: 2020-02-21
        endDate: 2020-02-21
      - name: Data limite para lançamento de classificações no Portal Académico (época especial)
        startDate: 2020-03-14
        endDate: 2020-03-14
```

A particular note to `curricularTerm` which indicates the academic semesters afected by the specific events.



# Resources
* [List of Academic Institutions in Portugal](https://www.dges.gov.pt/guias/indest.asp)
* [List of Higher Education Programmes in Portugal](https://www.dges.gov.pt/guias/indcurso.asp)