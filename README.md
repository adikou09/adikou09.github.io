<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>📖 Электронный журнал — Колледж</title>

<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: #eef2f5;
    color: #263238;
}

header {
    background: #263746;
    color: white;
    padding: 18px 25px;
    position: sticky;
    top: 0;
    z-index: 1000;
    box-shadow: 0 2px 10px rgba(0,0,0,.15);
}

.header-inner {
    max-width: 1500px;
    margin: auto;
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 15px;
}

header h1 {
    margin: 0;
    font-size: 24px;
}

.sync-status {
    font-size: 13px;
    padding: 7px 12px;
    border-radius: 20px;
    background: #607d8b;
}

.sync-online {
    background: #2e7d32;
}

.sync-offline {
    background: #c62828;
}

nav {
    background: #344955;
    position: sticky;
    top: 70px;
    z-index: 999;
    overflow-x: auto;
}

.nav-inner {
    max-width: 1500px;
    margin: auto;
    display: flex;
}

nav button {
    border: 0;
    background: transparent;
    color: white;
    padding: 15px 18px;
    cursor: pointer;
    white-space: nowrap;
    font-size: 14px;
}

nav button:hover,
nav button.active {
    background: #607d8b;
}

main {
    max-width: 1500px;
    margin: auto;
    padding: 25px;
}

.page {
    display: none;
}

.page.active {
    display: block;
}

.card {
    background: white;
    border-radius: 12px;
    padding: 20px;
    margin-bottom: 20px;
    box-shadow: 0 2px 8px rgba(0,0,0,.08);
}

.card h2,
.card h3 {
    margin-top: 0;
}

.grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    gap: 15px;
}

.stat {
    background: #f5f7f9;
    border-radius: 10px;
    padding: 18px;
    text-align: center;
}

.stat-number {
    font-size: 30px;
    font-weight: bold;
    color: #263746;
}

.stat-label {
    color: #607d8b;
    margin-top: 5px;
}

button,
input,
select,
textarea {
    font: inherit;
}

.btn {
    border: 0;
    border-radius: 7px;
    padding: 9px 14px;
    cursor: pointer;
    background: #455a64;
    color: white;
    margin: 3px;
}

.btn:hover {
    opacity: .9;
}

.btn-primary {
    background: #1976d2;
}

.btn-success {
    background: #2e7d32;
}

.btn-danger {
    background: #c62828;
}

.btn-warning {
    background: #ef6c00;
}

.btn-light {
    background: #eceff1;
    color: #263238;
}

input,
select,
textarea {
    border: 1px solid #cfd8dc;
    border-radius: 6px;
    padding: 8px;
    width: 100%;
    background: white;
}

textarea {
    min-height: 90px;
    resize: vertical;
}

.form-group {
    margin-bottom: 12px;
}

.form-group label {
    display: block;
    font-weight: bold;
    margin-bottom: 5px;
}

.table-wrap {
    overflow-x: auto;
}

table {
    width: 100%;
    border-collapse: collapse;
    min-width: 900px;
}

th,
td {
    border: 1px solid #d7dee2;
    padding: 9px;
    vertical-align: middle;
}

th {
    background: #37474f;
    color: white;
    position: sticky;
    top: 0;
    z-index: 2;
}

tbody tr:nth-child(even) {
    background: #f8fafb;
}

.student-name {
    font-weight: bold;
    min-width: 240px;
}

.late-critical {
    text-decoration: underline;
    text-decoration-color: red;
    text-decoration-thickness: 3px;
    text-underline-offset: 4px;
}

.critical-box {
    color: #c62828;
    font-weight: bold;
    font-size: 12px;
}

.lesson-box {
    min-width: 300px;
    background: #f7f9fa;
    border-radius: 8px;
    padding: 8px;
}

.lesson-title {
    font-weight: bold;
    background: #455a64;
    color: white;
    padding: 7px;
    border-radius: 5px;
    margin-bottom: 7px;
    text-align: center;
}

.lesson-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 5px;
}

.lesson-grid label {
    font-size: 11px;
    color: #607d8b;
}

.lesson-grid .full {
    grid-column: 1 / -1;
}

.lesson-status {
    width: 100%;
}

.toilet-control {
    display: flex;
    align-items: center;
    gap: 5px;
}

.toilet-control input {
    width: 65px;
    text-align: center;
}

.duty-person {
    border: 2px solid #cfd8dc;
    border-radius: 10px;
    padding: 15px;
    margin-bottom: 10px;
}

.duty-person.completed {
    border-color: #2e7d32;
    background: #f1f8f1;
}

.duty-person.skipped {
    border-color: #ef6c00;
    background: #fff7ef;
}

.duty-name {
    font-size: 18px;
    font-weight: bold;
}

.badge {
    display: inline-block;
    padding: 4px 8px;
    border-radius: 15px;
    font-size: 12px;
    background: #eceff1;
}

.badge-green {
    background: #c8e6c9;
    color: #1b5e20;
}

.badge-red {
    background: #ffcdd2;
    color: #b71c1c;
}

.badge-orange {
    background: #ffe0b2;
    color: #e65100;
}

.student-card {
    border: 1px solid #d7dee2;
    border-radius: 10px;
    padding: 15px;
    margin-bottom: 10px;
}

.student-card h3 {
    margin: 0 0 10px;
}

.alert {
    border-left: 5px solid #c62828;
    background: #ffebee;
    padding: 12px;
    margin-bottom: 10px;
    border-radius: 5px;
}

.success-message {
    border-left: 5px solid #2e7d32;
    background: #e8f5e9;
    padding: 12px;
    border-radius: 5px;
}

.empty {
    color: #78909c;
    text-align: center;
    padding: 25px;
}

.calendar {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 5px;
}

.calendar-day {
    min-height: 90px;
    border: 1px solid #cfd8dc;
    border-radius: 6px;
    padding: 7px;
    background: white;
}

.calendar-day.today {
    border: 2px solid #1976d2;
}

.calendar-day.has-event {
    background: #fff8e1;
}

.calendar-head {
    font-weight: bold;
    text-align: center;
    padding: 8px;
}

.calendar-event {
    font-size: 11px;
    margin-top: 5px;
    padding: 4px;
    background: #ffe082;
    border-radius: 4px;
}

.toast {
    position: fixed;
    right: 20px;
    bottom: 20px;
    background: #263238;
    color: white;
    padding: 14px 18px;
    border-radius: 8px;
    z-index: 5000;
    display: none;
    box-shadow: 0 4px 15px rgba(0,0,0,.25);
}

.small {
    font-size: 12px;
    color: #607d8b;
}

.danger-text {
    color: #c62828;
    font-weight: bold;
}

@media(max-width:700px) {

    main {
        padding: 12px;
    }

    header {
        padding: 13px;
    }

    header h1 {
        font-size: 18px;
    }

    .header-inner {
        flex-direction: column;
        align-items: flex-start;
    }

    nav {
        top: 105px;
    }

    nav button {
        padding: 12px;
    }
}

/* ===== COLLEGE V2 ===== */
.role-pill{display:inline-block;padding:5px 9px;border-radius:14px;background:#e3f2fd;color:#0d47a1;font-size:12px;font-weight:bold}.muted{color:#607d8b;font-size:13px}.toolbar{display:flex;gap:8px;align-items:center;justify-content:space-between;flex-wrap:wrap}.override-row{background:#fff8e1!important}.holiday-row{background:#ffebee!important}.emergency-overlay{position:fixed;inset:0;background:rgba(170,0,0,.96);color:#fff;z-index:10000;display:none;align-items:center;justify-content:center;text-align:center;padding:30px}.emergency-overlay.active{display:flex}.emergency-overlay .inner{max-width:900px}.emergency-overlay h1{font-size:clamp(32px,7vw,76px);margin:0 0 20px}.emergency-overlay p{font-size:clamp(18px,3vw,30px);white-space:pre-wrap}.auth-overlay{position:fixed;inset:0;background:rgba(20,30,40,.82);z-index:9000;display:none;align-items:center;justify-content:center;padding:20px}.auth-overlay.active{display:flex}.auth-box{width:min(520px,100%);background:#fff;border-radius:14px;padding:24px;box-shadow:0 20px 60px rgba(0,0,0,.35)}.auth-box h2{margin-top:0}.danger-row{background:#ffebee!important}
</style>
</head>

<body>

<header>
    <div class="header-inner">
        <div style="display:flex;align-items:center;gap:12px;flex-wrap:wrap"><h1 style="margin:0">📖 Электронный журнал</h1><span id="headerGroupLabel" class="badge" style="background:#607d8b;color:white">Группа 3 — ПОВТАС</span></div>
        <div style="display:flex;align-items:center;gap:8px;flex-wrap:wrap;justify-content:flex-end"><select id="activeGroupSelect" style="width:auto;min-width:220px;color:#263238" onchange="switchCollegeGroup(this.value)"><option value="G3">Группа 3 — ПОВТАС</option><option value="G1">Группа 1 — Менеджмент (по отраслям)</option><option value="G2">Группа 2 — Менеджмент (по отраслям)</option></select><button class="btn btn-light" style="margin:0" onclick="openLogin()">🔐 Вход</button><span id="authStatus" class="badge" style="background:#eceff1;color:#263238">Гостевой режим</span><div id="syncStatus" class="sync-status">Подключение...</div></div>
    </div>
</header>

<nav>
    <div class="nav-inner">

        <button onclick="showPage('home', this)" class="active">
            🏠 Главная
        </button>

        <button onclick="showPage('journal', this)">
            📖 Журнал
        </button>

        <button onclick="showPage('duties', this)">
            👮 Дежурства
        </button>

        <button onclick="showPage('students', this)">
            👥 Студенты
        </button>

        <button onclick="showPage('statistics', this)">
            📊 Статистика
        </button>

        <button onclick="showPage('history', this)">
            📜 История
        </button>

        <button onclick="showPage('notes', this)">
            📝 Заметки
        </button>

        <button onclick="showPage('calendar', this)">
            📅 Календарь
        </button>

        <button onclick="showPage('schedule', this)">📚 Расписание</button>
        <button onclick="showPage('grades', this)">🎓 Оценки</button>
        <button onclick="showPage('events', this)">📢 События</button>
        <button onclick="showPage('profile', this)">👤 Профиль</button>

        <button onclick="showPage('settings', this)">
            ⚙️ Настройки
        </button>

    </div>
</nav>

<main>

<section id="home" class="page active">

    <div class="card">

        <h2>Добро пожаловать 👋</h2>

        <p>
            Общий электронный журнал группы.
            Все изменения синхронизируются между компьютерами.
        </p>

    </div>

    <div class="card">

        <h2>
            📅 Сегодня:
            <span id="homeDate"></span>
        </h2>

        <div class="grid">

            <div class="stat">
                <div class="stat-number" id="homeStudents">0</div>
                <div class="stat-label">
                    Студентов
                </div>
            </div>

            <div class="stat">
                <div class="stat-number" id="homePresent">0</div>
                <div class="stat-label">
                    Присутствуют
                </div>
            </div>

            <div class="stat">
                <div class="stat-number" id="homeAbsent">0</div>
                <div class="stat-label">
                    Отсутствуют
                </div>
            </div>

            <div class="stat">
                <div class="stat-number" id="homeLate">0</div>
                <div class="stat-label">
                    Опоздали
                </div>
            </div>

        </div>

    </div>

    <div class="card">

        <h2>👮 Дежурные сегодня</h2>

        <div id="homeDuties"></div>

    </div>

    <div class="card">

        <h2>🔴 Важные уведомления</h2>

        <div id="homeAlerts"></div>

    </div>

</section>


<section id="journal" class="page">

    <div class="card">

        <h2>📖 Журнал посещаемости</h2>

        <div class="grid">

            <div class="form-group">

                <label>Дата</label>

                <input
                    type="date"
                    id="journalDate"
                    onchange="renderJournal()"
                >

            </div>

            <div>

                <label>&nbsp;</label>

                <button
                    class="btn btn-primary"
                    onclick="setToday()"
                >
                    Сегодня
                </button>

            </div>

        </div>

        <p class="small">

            1-я пара: <b>08:00–09:20</b> | 2-я пара: <b>09:30–10:50</b> | 3-я пара: <b>11:00–12:20</b> | 4-я пара: <b>13:25–14:45</b>

        </p>

        <p class="small">

            Если опоздание больше 20 минут —
            <span class="danger-text">
                ФИО подчёркивается красным
            </span>
            и студент получает дежурство вне очереди.

        </p>

    </div>

    <div class="card">

        <div class="table-wrap">

            <table>

                <thead>

                <tr>

                    <th>№</th>

                    <th>ФИО</th>

                    <th>
                        1-я пара<br>
                        08:45–10:05
                    </th>

                    <th>
                        2-я пара<br>
                        10:15–11:35
                    </th>

                    <th>
                        3-я пара<br>
                        11:55–13:15
                    </th>

                    <th>
                        Причина / примечание
                    </th>

                </tr>

                </thead>

                <tbody id="journalBody"></tbody>

            </table>

        </div>

    </div>

</section>


<section id="duties" class="page">

    <div class="card">

        <h2>👮 Дежурства сегодня</h2>

        <p>
            Каждый день назначаются <b>2 человека</b>.
            Следующая пара начинается после того,
            как оба человека завершили или пропустили
            своё дежурство.
        </p>

        <div id="todayDuties"></div>

    </div>

    <div class="card">

        <h2>📋 Очередь дежурств</h2>

        <div id="dutyQueue"></div>

    </div>

    <div class="card">

        <h2>⚠️ Вне очереди</h2>

        <div id="extraDuties"></div>

    </div>

</section>


<section id="students" class="page">

    <div class="card">

        <h2>👥 Студенты</h2>

        <div class="grid">

            <div class="form-group">

                <label>ФИО</label>

                <input
                    type="text"
                    id="newStudentName"
                    placeholder="Введите ФИО"
                >

            </div>

            <div>

                <label>&nbsp;</label>

                <button
                    class="btn btn-success"
                    onclick="addStudent()"
                >
                    ➕ Добавить
                </button>

            </div>

        </div>

    </div>

    <div class="card">

        <div id="studentsList"></div>

    </div>

</section>


<section id="statistics" class="page">

    <div class="card">

        <h2>📊 Статистика</h2>

        <div class="grid">

            <div class="stat">
                <div class="stat-number" id="statTotal">0</div>
                <div class="stat-label">
                    Всего отметок
                </div>
            </div>

            <div class="stat">
                <div class="stat-number" id="statPresent">0</div>
                <div class="stat-label">
                    Присутствие
                </div>
            </div>

            <div class="stat">
                <div class="stat-number" id="statAbsent">0</div>
                <div class="stat-label">
                    Отсутствия
                </div>
            </div>

            <div class="stat">
                <div class="stat-number" id="statLate">0</div>
                <div class="stat-label">
                    Опоздания
                </div>
            </div>

            <div class="stat">
                <div class="stat-number" id="statCritical">0</div>
                <div class="stat-label">
                    Опоздание >20 мин
                </div>
            </div>

            <div class="stat">
                <div class="stat-number" id="statToilet">0</div>
                <div class="stat-label">
                    Выходы в туалет
                </div>
            </div>

        </div>

    </div>

    <div class="card">

        <h2>Статистика по студентам</h2>

        <div class="table-wrap">

            <table>

                <thead>

                <tr>

                    <th>№</th>
                    <th>ФИО</th>
                    <th>Присутствий</th>
                    <th>Отсутствий</th>
                    <th>Опозданий</th>
                    <th>>20 минут</th>
                    <th>Туалет</th>

                </tr>

                </thead>

                <tbody id="statisticsBody"></tbody>

            </table>

        </div>

    </div>

</section>


<section id="history" class="page">

    <div class="card">

        <h2>📜 История</h2>

        <div id="historyList"></div>

    </div>

</section>


<section id="notes" class="page">

    <div class="card">

        <h2>📝 Заметки</h2>

        <div class="form-group">

            <label>Новая заметка</label>

            <textarea
                id="newNoteText"
                placeholder="Введите заметку..."
            ></textarea>

        </div>

        <button
            class="btn btn-primary"
            onclick="addNote()"
        >
            Добавить заметку
        </button>

    </div>

    <div class="card">

        <div id="notesList"></div>

    </div>

</section>


<section id="calendar" class="page">

    <div class="card">

        <h2>📅 Календарь</h2>

        <div class="grid">

            <button
                class="btn"
                onclick="changeMonth(-1)"
            >
                ← Предыдущий
            </button>

            <h3
                id="calendarTitle"
                style="text-align:center;"
            ></h3>

            <button
                class="btn"
                onclick="changeMonth(1)"
            >
                Следующий →
            </button>

        </div>

        <div
            class="calendar"
            id="calendarGrid"
        ></div>

    </div>

    <div class="card">

        <h2>
            🎂 Добавить день рождения / напоминание
        </h2>

        <div class="grid">

            <div class="form-group">

                <label>Дата</label>

                <input
                    type="date"
                    id="reminderDate"
                >

            </div>

            <div class="form-group">

                <label>Текст</label>

                <input
                    type="text"
                    id="reminderText"
                    placeholder="Например: День рождения Алихана"
                >

            </div>

        </div>

        <button
            class="btn btn-primary"
            onclick="addReminder()"
        >
            Добавить
        </button>

    </div>

    <div class="card">

        <h2>🔔 Напоминания</h2>

        <div id="remindersList"></div>

    </div>

</section>


<section id="settings" class="page">

    <div class="card">

        <h2>⚙️ Настройки</h2>

        <div class="form-group">

            <label>Название группы</label>

            <input id="settingGroupName">

        </div>

        <div class="form-group">

            <label>
                Порог опоздания для дежурства вне очереди
            </label>

            <input
                type="number"
                id="settingLateLimit"
                min="1"
            >

        </div>

        <button
            class="btn btn-success"
            onclick="saveSettings()"
        >
            💾 Сохранить настройки
        </button>

    </div>

    <div class="card">

        <h2>☁️ Синхронизация</h2>

        <p>
            Данные журнала хранятся в общей облачной базе.
            Поэтому один и тот же журнал можно открыть
            на нескольких компьютерах.
        </p>

        <button
            class="btn btn-primary"
            onclick="loadFromCloud()"
        >
            ☁️ Загрузить данные из облака
        </button>

        <button
            class="btn btn-success"
            onclick="saveToCloud(true)"
        >
            ☁️ Принудительно сохранить
        </button>

    </div>

    <div class="card">

        <h2>💾 Резервная копия</h2>

        <button
            class="btn btn-primary"
            onclick="exportData()"
        >
            📤 Экспорт
        </button>

        <button
            class="btn btn-warning"
            onclick="document.getElementById('importFile').click()"
        >
            📥 Импорт
        </button>

        <input
            type="file"
            id="importFile"
            accept=".json"
            style="display:none"
            onchange="importData(event)"
        >

    </div>

</section>


<section id="schedule" class="page">
  <div class="card">
    <h2>📚 Расписание</h2>
    <div class="grid">
      <div class="form-group"><label>Дата</label><input type="date" id="scheduleDate"></div>
      <div class="form-group"><label>Группа</label><select id="scheduleGroup" onchange="renderCollegeSchedule()"></select></div>
    </div>
    <div id="scheduleHolidayBox"></div>
    <div class="table-wrap"><table><thead><tr><th>№</th><th>Время</th><th>Предмет</th><th>Преподаватель</th><th>Кабинет</th><th>Действие</th></tr></thead><tbody id="scheduleBody"></tbody></table></div>
  </div>
  <div class="card" id="scheduleEditCard">
    <h3>✏️ Разовая замена пары</h3>
    <div class="grid">
      <div class="form-group"><label>Дата</label><input type="date" id="overrideDate"></div>
      <div class="form-group"><label>Пара</label><select id="overrideLesson"><option value="1">1-я</option><option value="2">2-я</option><option value="3">3-я</option><option value="4">4-я</option></select></div>
      <div class="form-group"><label>Предмет</label><input id="overrideSubject" placeholder="Например: Алгебра"></div>
      <div class="form-group"><label>Преподаватель</label><input id="overrideTeacher"></div>
      <div class="form-group"><label>Кабинет</label><input id="overrideRoom"></div>
      <div class="form-group"><label>Причина</label><input id="overrideReason" placeholder="Например: замена Биологии"></div>
    </div>
    <button class="btn btn-primary" onclick="saveScheduleOverride()">💾 Сохранить замену</button>
    <button class="btn btn-danger" onclick="cancelScheduleOverride()">❌ Отменить выбранную пару</button>
  </div>
  <div class="card" id="holidayEditCard">
    <h3>🎉 Нерабочий / праздничный день</h3>
    <div class="grid"><input type="date" id="holidayDate"><input id="holidayTitle" placeholder="Например: День независимости"></div>
    <button class="btn btn-warning" onclick="saveHoliday()">Добавить выходной</button>
  </div>
</section>

<section id="grades" class="page">
  <div class="card">
    <h2>🎓 Оценки</h2>
    <div class="grid">
      <div class="form-group"><label>Группа</label><select id="gradesGroup" onchange="renderGrades()"></select></div>
      <div class="form-group"><label>Предмет</label><input id="gradeSubject" placeholder="Например: Математика"></div>
      <div class="form-group"><label>Студент</label><select id="gradeStudent"></select></div>
      <div class="form-group"><label>Оценка</label><select id="gradeValue"><option>5</option><option>4</option><option>3</option><option>2</option><option>1</option></select></div>
      <div class="form-group"><label>Дата</label><input type="date" id="gradeDate"></div>
      <div class="form-group"><label>Тема</label><input id="gradeTitle" placeholder="Контрольная / ответ / экзамен"></div>
    </div>
    <button class="btn btn-success" onclick="saveGrade()">💾 Поставить оценку</button>
    <div class="small">Учитель и администратор могут добавлять оценки. Без авторизации страница доступна для просмотра локальных данных.</div>
  </div>
  <div class="card"><div class="table-wrap"><table><thead><tr><th>Студент</th><th>Предмет</th><th>Тема</th><th>Оценка</th><th>Дата</th><th>Средний балл</th><th></th></tr></thead><tbody id="gradesBody"></tbody></table></div></div>
</section>

<section id="events" class="page">
  <div class="card">
    <h2>📢 Объявления и срочные сборы</h2>
    <div class="grid">
      <div class="form-group"><label>Заголовок</label><input id="eventTitle" placeholder="Например: Сбор в актовом зале"></div>
      <div class="form-group"><label>Для группы</label><select id="eventGroup"></select></div>
      <div class="form-group"><label>Начало</label><input type="datetime-local" id="eventStart"></div>
      <div class="form-group"><label>Окончание</label><input type="datetime-local" id="eventEnd"></div>
    </div>
    <div class="form-group"><label>Текст</label><textarea id="eventBody" placeholder="Текст объявления"></textarea></div>
    <label style="display:flex;gap:8px;align-items:center;margin:8px 0"><input type="checkbox" id="eventEmergency" style="width:auto"> ⚠️ Срочное уведомление</label>
    <label style="display:flex;gap:8px;align-items:center;margin:8px 0"><input type="checkbox" id="eventFullscreen" style="width:auto"> 🖥️ Показать большим экраном</label>
    <button class="btn btn-danger" onclick="saveEvent()">📢 Опубликовать</button>
  </div>
  <div class="card"><div id="eventsList"></div></div>
</section>

<section id="profile" class="page">
  <div class="card">
    <h2>👤 Профиль и доступ</h2>
    <div id="profileInfo"></div>
    <div id="loginInfo" class="success-message" style="display:none"></div>
  </div>
  <div class="card" id="passwordCard">
    <h3>🔑 Смена пароля</h3>
    <div class="grid"><input type="password" id="newPassword" placeholder="Новый пароль (минимум 6 символов)"><input type="password" id="newPassword2" placeholder="Повторите пароль"></div>
    <button class="btn btn-primary" onclick="changePassword()">Изменить пароль</button>
    <button class="btn btn-light" onclick="signOutCollege()">Выйти</button>
  </div>
  <div class="card" id="adminCard" style="display:none">
    <h3>🛡️ Администратор</h3>
    <p class="small">Аккаунты создаются вручную в Supabase Auth. Этот проект не создаёт отдельные аккаунты для 54 студентов.</p>
    <div class="grid"><input id="resetUserId" placeholder="UUID пользователя Supabase"><input id="resetPassword" type="password" placeholder="Новый пароль"></div>
    <button class="btn btn-danger" onclick="adminResetPassword()">Сбросить пароль</button>
  </div>
</section>

</main>

<div id="toast" class="toast"></div>





<div id="collegeAuthOverlay" class="auth-overlay">
  <div class="auth-box">
    <div class="toolbar"><h2>🔐 Вход в журнал</h2><button class="btn btn-light" onclick="closeLogin()">✕</button></div>
    <p class="small">Отдельные аккаунты для каждого студента не создаются. Используются общие ролевые аккаунты: Ученик, Учитель, Администратор.</p>
    <div class="form-group"><label>Роль</label><select id="loginRole"><option value="student">Ученик</option><option value="teacher">Учитель</option><option value="admin">Администратор</option></select></div>
    <div class="form-group"><label>Email</label><input type="email" id="loginEmail" placeholder="Введите email аккаунта"></div>
    <div class="form-group"><label>Пароль</label><input type="password" id="loginPassword" placeholder="Пароль"></div>
    <button class="btn btn-primary" style="width:100%" onclick="signInCollege()">Войти</button>
    <button class="btn btn-light" style="width:100%" onclick="closeLogin()">Продолжить без входа</button>
    <div id="loginError" class="alert" style="display:none;margin-top:12px"></div>
  </div>
</div>
<div id="emergencyOverlay" class="emergency-overlay"><div class="inner"><h1>⚠️ СРОЧНОЕ ОБЪЯВЛЕНИЕ</h1><h2 id="emergencyTitle"></h2><p id="emergencyBody"></p><button class="btn btn-light" onclick="closeEmergency()">Понятно</button></div></div>

<script>
/* =========================================================
   ЭЛЕКТРОННЫЙ ЖУРНАЛ — COLLEGE V2
   На основе исходного index_fixed.html.
   Студенты НЕ создаются как Auth-пользователи.
   Облачные данные журнала хранятся в group3_journal.data.
   Ролевые аккаунты (Ученик/Учитель/Админ) создаются вручную.
   ========================================================= */

const COLLEGE_SUPABASE_URL = "https://rufhsyyljvjyqdmhvpam.supabase.co";
const COLLEGE_SUPABASE_KEY = "sb_publishable_v2nQMnj57S6lT_KKY7zJuw_YR2okvFj";
const collegeSupabase = window.supabase?.createClient(COLLEGE_SUPABASE_URL, COLLEGE_SUPABASE_KEY) || null;

const COLLEGE_GROUPS = {
  G1:{code:"G1",name:"Группа 1 — Менеджмент (по отраслям)",short:"Группа 1",specialty:"Менеджмент (по отраслям)",students:[
    "Абдылдаева Айназик Калыгуловна","Адилов Абдулла Холназвич","Айдаралиева Арууке Улановна","Акжолбеков Бакай Айманбекович","Акылбеков Мирзат Русланович","Алапаев Умар Айбекович","Алмазов Амир Алмазович","Алымбеков Алим Алымбекович","Асанакунов Раззак Каныбекович","Базарбаева Асема Белековна","Байышбекова Сабина Талгатовна","Бекматбекова Айбийке Уланбековна","Бирюкова Ксения Владиславовна","Ботоканов Эмир Адылбекович","Джумакеев Марсел Мирбекович","Дильмухамедов Мухаммад Амин Дильмухамедович","Кабылбеков Алинур Бактыбекович","Казбекова Айдай Урматовна"]},
  G2:{code:"G2",name:"Группа 2 — Менеджмент (по отраслям)",short:"Группа 2",specialty:"Менеджмент (по отраслям)",students:[
    "Абасов Шерзатбек Тургуналиевич","Асаналиев Асланбек Ажыбекович","Камчыбекова Бегимай Суюмкуловна","Каныбеков Абдурахим Курсанбекович","Керималиев Умар Жуман Казбекович","Консулбекова Айдана Айтбековна","Курманов Амаль Ильясович","Марсова Аделя Нурлановна","Пазылов Нурсултан Абдыразакович","Рысбек Кызы Раяна","Суванкулов Азат Суванкулович","Суйналиева Айлин Уланбековна","Султанбеков Амантур Миржанович","Таабалдиева Мээримай Дамирбековна","Талантбекова Тумар Рустамовна","Шавкетов Измир Лучезарович","Шаршекеев Рыспек Телекович","Эркинбеков Абдулазиз Эркинбекович"]},
  G3:{code:"G3",name:"Группа 3 — ПОВТАС",short:"Группа 3",specialty:"ПОВТАС",students:[
    "Абдужапаров Ибрагим Кубанычбекович","Абдыкадыров Алихан Чыныбекович","Джумагулов Тимур Алмазбекович","Жаркынбаев Тимурлан Кубанычбекович","Закиров Камиль Шамилевич","Икболиддинов Бехруз Икболиддинович","Казбеков Адилет Урматович","Крамаренко Руслан Дмитриевич","Курманбеков Хамза Мунарбекович","Курманбеков Эмиль Баатырбекович","Миршакиров Азирет Нурланович","Муктаров Арген Афтандилович","Мурзараимов Алинур Нуртилекович","Сулайманов Улар Уланбекович","Таласбеков Абдуллах Таласбекович","Таласбеков Абдурахман Таласбекович","Тороев Амир Уланбекович","Хакимов Санжар Рахимжанович"]}
};
const LESSONS_V2=[
 {id:1,name:"1-я пара",start:"08:00",end:"09:20"},
 {id:2,name:"2-я пара",start:"09:30",end:"10:50"},
 {id:3,name:"3-я пара",start:"11:00",end:"12:20"},
 {id:4,name:"4-я пара",start:"13:25",end:"14:45"}
];
const CLOUD_KEY="college_journal_v2";
let collegeData={version:2,activeGroup:"G3",groups:{},overrides:{},holidays:[],events:[],grades:[],reminders:[],notes:[],history:[],scheduleSubjects:{G1:["Менеджмент","Экономика","Математика","Информатика"],G2:["Менеджмент","Право","Экономика","Информатика"],G3:["Программирование","Базы данных","Сетевые технологии","Практика"]}};
let collegeSession=null,collegeProfile=null,collegeRealtime=null;let calendarCursor=new Date();
const roleNames={student:"Ученик",teacher:"Учитель",admin:"Администратор"};
const esc=v=>String(v??"").replace(/[&<>'"]/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;',"'":'&#39;','"':'&quot;'}[c]));
const dateKey=()=>{const d=new Date();return `${d.getFullYear()}-${String(d.getMonth()+1).padStart(2,'0')}-${String(d.getDate()).padStart(2,'0')}`};
const uid=p=>`${p}_${Date.now().toString(36)}_${Math.random().toString(36).slice(2,8)}`;
const roleCanWrite=()=>!!collegeProfile&&["teacher","admin"].includes(collegeProfile.role);
const currentGroupCode=()=>collegeData.activeGroup||"G3";
const groupInfo=code=>COLLEGE_GROUPS[code]||COLLEGE_GROUPS.G3;
const studentsFor=code=>{const g=groupInfo(code);const existing=collegeData.groups[code]?.students;return (existing&&existing.length?existing:g.students.map((name,i)=>({id:`${code}_${String(i+1).padStart(2,'0')}`,name})));};
function ensureGroups(){for(const [code,g] of Object.entries(COLLEGE_GROUPS)){if(!collegeData.groups[code]) collegeData.groups[code]={students:g.students.map((name,i)=>({id:`${code}_${String(i+1).padStart(2,'0')}`,name})),attendance:{},duties:{currentIndex:0,daily:{}},notes:[]};else if(!Array.isArray(collegeData.groups[code].students)||collegeData.groups[code].students.length!==18) collegeData.groups[code].students=g.students.map((name,i)=>({id:`${code}_${String(i+1).padStart(2,'0')}`,name}));}}
function normalizeCollegeData(raw){if(raw&&raw.version===2){collegeData={...collegeData,...raw};ensureGroups();return;} if(raw&&Array.isArray(raw.students)){collegeData.groups.G3={students:raw.students.map((s,i)=>({id:s.id||`G3_${String(i+1).padStart(2,'0')}`,name:s.name||String(s)})),attendance:raw.attendance||{},duties:raw.duties||{currentIndex:0,daily:{}},notes:raw.notes||[]};collegeData.history=raw.history||[];collegeData.reminders=raw.reminders||[];collegeData.activeGroup="G3";} ensureGroups();}
function activeGroupData(){ensureGroups();return collegeData.groups[currentGroupCode()]}
function attendanceRecord(date,studentId,lessonNo,create=true){const g=activeGroupData();g.attendance[date]??={};g.attendance[date][studentId]??={};g.attendance[date][studentId].lessons??={};if(create)g.attendance[date][studentId].lessons[lessonNo]??={status:"absent",arrival:"",reason:"",toilet:0,late:0};return g.attendance[date][studentId].lessons[lessonNo]}
function critical(date,studentId,lessonNo){const r=attendanceRecord(date,studentId,lessonNo,false);return Number(r?.late||0)>Number(collegeData.lateLimit||20)}
function lateMinutes(lesson,arrival){if(!arrival)return 0;const [h,m]=lesson.start.split(':').map(Number),[ah,am]=arrival.split(':').map(Number);return Math.max(0,(ah*60+am)-(h*60+m))}
function updateSync(text,online=false){const e=document.getElementById('syncStatus');if(e){e.textContent=text;e.classList.toggle('sync-online',online);e.classList.toggle('sync-offline',!online&&/ошиб|нет|offline/i.test(text));}}
function saveLocal(){localStorage.setItem(CLOUD_KEY,JSON.stringify(collegeData))}
function loadLocal(){try{const x=JSON.parse(localStorage.getItem(CLOUD_KEY)||'null');if(x)normalizeCollegeData(x)}catch(e){console.warn(e)}}
async function cloudSave(){saveLocal();if(!collegeSupabase){updateSync('💾 Локальный режим');return false}try{updateSync('☁️ Сохранение...');const {error}=await collegeSupabase.from('group3_journal').upsert({id:'main',data:collegeData,updated_at:new Date().toISOString()},{onConflict:'id'});if(error)throw error;updateSync('☁️ Облако подключено',true);return true}catch(e){console.error(e);updateSync('⚠️ Ошибка облака');return false}}
async function cloudLoad(){if(!collegeSupabase){updateSync('💾 Локальный режим');return}try{updateSync('☁️ Загрузка...');const {data,error}=await collegeSupabase.from('group3_journal').select('data').eq('id','main').maybeSingle();if(error)throw error;if(data?.data)normalizeCollegeData(data.data);else await cloudSave();saveLocal();updateSync('☁️ Облако подключено',true);refreshCollege();}catch(e){console.warn('Cloud load:',e);updateSync('💾 Локальные данные');}}
async function loadAuth(){if(!collegeSupabase)return;try{const {data}=await collegeSupabase.auth.getSession();collegeSession=data.session||null;if(collegeSession){const {data:p}=await collegeSupabase.from('profiles').select('*').eq('id',collegeSession.user.id).maybeSingle();collegeProfile=p||null;}renderAuth();}catch(e){console.warn(e)}}
function renderAuth(){const a=document.getElementById('authStatus'),p=document.getElementById('profileInfo'),admin=document.getElementById('adminCard');if(collegeSession){a.textContent=`${roleNames[collegeProfile?.role]||'Пользователь'}: ${collegeProfile?.full_name||collegeSession.user.email}`;a.style.background='#c8e6c9';a.style.color='#1b5e20';p.innerHTML=`<p><b>Аккаунт:</b> ${esc(collegeSession.user.email)}</p><p><b>Роль:</b> ${esc(roleNames[collegeProfile?.role]||'Не указана')}</p>${collegeProfile?.group_id?'<p class="small">Группа назначена в профиле Supabase.</p>':''}`;admin.style.display=collegeProfile?.role==='admin'?'block':'none';}else{a.textContent='Гостевой режим';a.style.background='#eceff1';a.style.color='#263238';p.innerHTML='<p>Сейчас журнал открыт без аккаунта. Посещаемость, оценки и настройки сохраняются локально и в облако согласно доступным RLS-политикам.</p>';admin.style.display='none'}}
function openLogin(){document.getElementById('collegeAuthOverlay').classList.add('active');document.getElementById('loginError').style.display='none'}function closeLogin(){document.getElementById('collegeAuthOverlay').classList.remove('active')}
async function signInCollege(){const email=document.getElementById('loginEmail').value.trim(),password=document.getElementById('loginPassword').value,role=document.getElementById('loginRole').value,err=document.getElementById('loginError');err.style.display='none';if(!collegeSupabase){err.textContent='Supabase не подключён.';err.style.display='block';return}try{const {data,error}=await collegeSupabase.auth.signInWithPassword({email,password});if(error)throw error;collegeSession=data.session;const {data:p,error:pe}=await collegeSupabase.from('profiles').select('*').eq('id',collegeSession.user.id).maybeSingle();if(pe)throw pe;collegeProfile=p;if(!collegeProfile){await collegeSupabase.auth.signOut();throw new Error('Для этого аккаунта нет профиля в public.profiles. Создай профиль вручную.')}if(collegeProfile.role!==role){await collegeSupabase.auth.signOut();throw new Error(`Выбрана роль «${roleNames[role]}», но аккаунт имеет роль «${roleNames[collegeProfile.role]||collegeProfile.role}».`)}closeLogin();renderAuth();applyPermissions();showToast('Вход выполнен');}catch(e){console.error(e);err.textContent=e.message||String(e);err.style.display='block'}}
async function signOutCollege(){if(collegeSupabase)await collegeSupabase.auth.signOut();collegeSession=null;collegeProfile=null;renderAuth();applyPermissions();showToast('Вы вышли из аккаунта')}
async function changePassword(){if(!collegeSession){openLogin();return}const a=document.getElementById('newPassword').value,b=document.getElementById('newPassword2').value;if(a.length<6||a!==b){showToast('Пароли не совпадают или короче 6 символов');return}const {error}=await collegeSupabase.auth.updateUser({password:a});if(error)showToast(error.message);else{document.getElementById('newPassword').value='';document.getElementById('newPassword2').value='';showToast('Пароль изменён')}}
async function adminResetPassword(){if(collegeProfile?.role!=='admin'){showToast('Нужна роль администратора');return}const uid=document.getElementById('resetUserId').value.trim(),password=document.getElementById('resetPassword').value;if(!uid||password.length<6){showToast('Укажи UUID пользователя и пароль от 6 символов');return}const {data:{session}}=await collegeSupabase.auth.getSession();try{const r=await fetch(`${COLLEGE_SUPABASE_URL}/functions/v1/admin-reset-password`,{method:'POST',headers:{Authorization:`Bearer ${session.access_token}`,apikey:COLLEGE_SUPABASE_KEY,'Content-Type':'application/json'},body:JSON.stringify({user_id:uid,password})});const j=await r.json();if(!r.ok)throw new Error(j.error||j.message||`HTTP ${r.status}`);showToast('Пароль сброшен')}catch(e){showToast(`Сброс не выполнен: ${e.message}`)}}
function switchCollegeGroup(code){collegeData.activeGroup=COLLEGE_GROUPS[code]?code:'G3';document.getElementById('activeGroupSelect').value=collegeData.activeGroup;document.getElementById('headerGroupLabel').textContent=groupInfo(collegeData.activeGroup).name;['scheduleGroup','gradesGroup','eventGroup'].forEach(id=>{const e=document.getElementById(id);if(e)e.value=collegeData.activeGroup});saveLocal();refreshCollege();}
function fillGroupSelects(){const opts=Object.values(COLLEGE_GROUPS).map(g=>`<option value="${g.code}">${esc(g.name)}</option>`).join('');for(const id of ['scheduleGroup','gradesGroup']){const e=document.getElementById(id);if(e){e.innerHTML=opts;e.value=collegeData.activeGroup||'G3'}}const eg=document.getElementById('eventGroup');if(eg){eg.innerHTML='<option value="">Все группы</option>'+opts;eg.value=collegeData.activeGroup||'G3'}const top=document.getElementById('activeGroupSelect');if(top)top.value=collegeData.activeGroup||'G3'}
function applyPermissions(){const staff=roleCanWrite();document.querySelectorAll('#scheduleEditCard,#holidayEditCard').forEach(e=>e.style.display=staff?'block':'none');const saveGrade=document.querySelector('#grades .btn-success');if(saveGrade)saveGrade.style.display=staff?'inline-block':'none';const saveEvent=document.querySelector('#events .btn-danger');if(saveEvent)saveEvent.style.display=staff?'inline-block':'none';}
function subjectsFor(code){return collegeData.scheduleSubjects[code]||['Предмет 1','Предмет 2','Предмет 3','Предмет 4']}
function overrideFor(code,date,no){return collegeData.overrides[`${code}_${date}_${no}`]||null}
function scheduleRow(code,date,no){const base=LESSONS_V2[no-1],o=overrideFor(code,date,no);const subj=o?.subject||subjectsFor(code)[no-1];return {...base,subject:subj,teacher:o?.teacher_name||'',room:o?.room||'',override:o,cancelled:!!o?.is_cancelled}}
function isHoliday(date){return collegeData.holidays.find(h=>h.date===date)}
function renderCollegeSchedule(){const date=document.getElementById('scheduleDate')?.value||dateKey(),code=document.getElementById('scheduleGroup')?.value||currentGroupCode(),body=document.getElementById('scheduleBody');if(!body)return;const holiday=isHoliday(date);document.getElementById('scheduleHolidayBox').innerHTML=holiday?`<div class="alert"><b>🎉 ${esc(holiday.title)}</b><br>${esc(holiday.description||'Учебные занятия отменены.')}</div>`:'';body.innerHTML=LESSONS_V2.map(l=>{const r=scheduleRow(code,date,l.id);return `<tr class="${r.cancelled?'holiday-row':r.override?'override-row':''}"><td>${l.id}</td><td>${l.start}–${l.end}</td><td><b>${r.cancelled?'Занятие отменено':esc(r.subject)}</b>${r.override&&!r.cancelled?'<div class="small">Разовая замена</div>':''}</td><td>${esc(r.teacher)}</td><td>${esc(r.room)}</td><td>${roleCanWrite()?`<button class="btn btn-light" onclick="loadOverrideForm('${date}',${l.id})">Изменить</button>`:'—'}</td></tr>`}).join('');}
function loadOverrideForm(date,no){document.getElementById('overrideDate').value=date;document.getElementById('overrideLesson').value=no;const o=overrideFor(document.getElementById('scheduleGroup').value,date,no);document.getElementById('overrideSubject').value=o?.subject||'';document.getElementById('overrideTeacher').value=o?.teacher_name||'';document.getElementById('overrideRoom').value=o?.room||'';document.getElementById('overrideReason').value=o?.reason||''}
async function saveScheduleOverride(){if(!roleCanWrite()){showToast('Только учитель или администратор');return}const code=document.getElementById('scheduleGroup').value,date=document.getElementById('overrideDate').value||dateKey(),no=Number(document.getElementById('overrideLesson').value),subject=document.getElementById('overrideSubject').value.trim();if(!subject){showToast('Укажи предмет');return}collegeData.overrides[`${code}_${date}_${no}`]={group_id:code,lesson_date:date,lesson_no:no,subject,teacher_name:document.getElementById('overrideTeacher').value.trim(),room:document.getElementById('overrideRoom').value.trim(),reason:document.getElementById('overrideReason').value.trim(),is_cancelled:false,updated_at:new Date().toISOString()};await cloudSave();renderCollegeSchedule();showToast('Замена сохранена')}
async function cancelScheduleOverride(){if(!roleCanWrite())return;const code=document.getElementById('scheduleGroup').value,date=document.getElementById('overrideDate').value||dateKey(),no=Number(document.getElementById('overrideLesson').value);collegeData.overrides[`${code}_${date}_${no}`]={group_id:code,lesson_date:date,lesson_no:no,is_cancelled:true,subject:'',updated_at:new Date().toISOString()};await cloudSave();renderCollegeSchedule();showToast('Пара отменена')}
async function saveHoliday(){if(!roleCanWrite())return;const date=document.getElementById('holidayDate').value,title=document.getElementById('holidayTitle').value.trim();if(!date||!title){showToast('Укажи дату и название');return}const old=collegeData.holidays.find(h=>h.date===date);if(old)old.title=title;else collegeData.holidays.push({id:uid('holiday'),date,title});await cloudSave();renderCollegeSchedule();renderCalendar();showToast('Выходной добавлен')}
function renderGrades(){const code=document.getElementById('gradesGroup')?.value||currentGroupCode(),students=studentsFor(code),sel=document.getElementById('gradeStudent');if(sel){sel.innerHTML=students.map(s=>`<option value="${s.id}">${esc(s.name)}</option>`).join('')}const rows=collegeData.grades.filter(g=>g.group===code).sort((a,b)=>String(b.date).localeCompare(String(a.date)));const avg={};for(const g of rows){avg[g.studentId]??=[];avg[g.studentId].push(Number(g.grade))}document.getElementById('gradesBody').innerHTML=rows.map(g=>{const a=avg[g.studentId];const av=a.reduce((x,y)=>x+y,0)/a.length;return `<tr><td>${esc(g.studentName)}</td><td>${esc(g.subject)}</td><td>${esc(g.title||'')}</td><td><b>${g.grade}</b></td><td>${g.date}</td><td>${av.toFixed(2)}</td><td>${roleCanWrite()?`<button class="btn btn-danger" onclick="deleteGrade('${g.id}')">Удалить</button>`:''}</td></tr>`}).join('')||'<tr><td colspan="7" class="empty">Оценок пока нет.</td></tr>'}
async function saveGrade(){if(!roleCanWrite()){showToast('Только учитель или администратор');return}const code=document.getElementById('gradesGroup').value,sid=document.getElementById('gradeStudent').value,s=studentsFor(code).find(x=>x.id===sid);if(!s||!document.getElementById('gradeSubject').value.trim())return showToast('Заполни студента и предмет');collegeData.grades.push({id:uid('grade'),group:code,studentId:sid,studentName:s.name,subject:document.getElementById('gradeSubject').value.trim(),title:document.getElementById('gradeTitle').value.trim(),grade:Number(document.getElementById('gradeValue').value),date:document.getElementById('gradeDate').value||dateKey()});await cloudSave();renderGrades();showToast('Оценка сохранена')}
async function deleteGrade(id){if(!roleCanWrite())return;collegeData.grades=collegeData.grades.filter(x=>x.id!==id);await cloudSave();renderGrades()}
function renderEvents(){const code=currentGroupCode(),list=collegeData.events.filter(e=>!e.group||e.group===code).sort((a,b)=>String(b.start).localeCompare(String(a.start)));document.getElementById('eventsList').innerHTML=list.map(e=>`<div class="card" style="border-left:5px solid ${e.emergency?'#c62828':'#1976d2'}"><div class="toolbar"><span class="badge ${e.emergency?'badge-red':'badge-green'}">${e.emergency?'⚠️ ЭКСТРЕННО':'📢 Объявление'}</span><span class="muted">${new Date(e.start).toLocaleString('ru-RU')}</span></div><h3>${esc(e.title)}</h3><p>${esc(e.body).replace(/\n/g,'<br>')}</p><div class="small">${e.group?esc(groupInfo(e.group).name):'Все группы'}</div>${roleCanWrite()?`<button class="btn btn-danger" onclick="deleteEvent('${e.id}')">Удалить</button>`:''}</div>`).join('')||'<div class="empty">Объявлений пока нет.</div>';const active=list.find(e=>e.emergency&&new Date(e.start)<=new Date()&&(!e.end||new Date(e.end)>=new Date()));if(active){document.getElementById('emergencyTitle').textContent=active.title;document.getElementById('emergencyBody').textContent=active.body;document.getElementById('emergencyOverlay').classList.add('active')}}
function closeEmergency(){document.getElementById('emergencyOverlay').classList.remove('active')}
async function saveEvent(){if(!roleCanWrite()){showToast('Только учитель или администратор');return}const title=document.getElementById('eventTitle').value.trim(),body=document.getElementById('eventBody').value.trim();if(!title||!body)return showToast('Заполни заголовок и текст');collegeData.events.push({id:uid('event'),title,body,start:document.getElementById('eventStart').value?new Date(document.getElementById('eventStart').value).toISOString():new Date().toISOString(),end:document.getElementById('eventEnd').value?new Date(document.getElementById('eventEnd').value).toISOString():null,group:document.getElementById('eventGroup').value||'',emergency:document.getElementById('eventEmergency').checked,fullscreen:document.getElementById('eventFullscreen').checked});await cloudSave();document.getElementById('eventTitle').value='';document.getElementById('eventBody').value='';renderEvents();showToast('Объявление опубликовано')}
async function deleteEvent(id){if(!roleCanWrite())return;collegeData.events=collegeData.events.filter(x=>x.id!==id);await cloudSave();renderEvents()}
function renderCollegeJournal(){const date=document.getElementById('journalDate')?.value||dateKey();const body=document.getElementById('journalBody');if(!body)return;const students=studentsFor(currentGroupCode());const th=document.querySelector('#journal thead tr');if(th){th.innerHTML='<th>№</th><th>ФИО</th>'+LESSONS_V2.map(l=>`<th>${l.name}<br>${l.start}–${l.end}</th>`).join('')+'<th>Причина / примечание</th>'}body.innerHTML=students.map((s,i)=>{const crit=LESSONS_V2.some(l=>critical(date,s.id,l.id));const cells=LESSONS_V2.map(l=>{const r=attendanceRecord(date,s.id,l.id);const c=Number(r.late)>Number(20);return `<td><div class="lesson-box"><div class="lesson-title">${l.start}–${l.end}</div><select class="lesson-status" onchange="setAttendanceV2('${date}','${s.id}',${l.id},this.value)"><option value="present" ${r.status==='present'?'selected':''}>Присутствует</option><option value="late" ${r.status==='late'?'selected':''}>Опоздал</option><option value="absent" ${r.status==='absent'?'selected':''}>Отсутствует</option></select><div class="lesson-grid"><label>Приход<input type="time" value="${esc(r.arrival||'')}" onchange="setArrivalV2('${date}','${s.id}',${l.id},this.value)"></label><label>Опоздание<input value="${Number(r.late||0)} мин" readonly></label><label class="full">Туалет <input type="number" min="0" value="${Number(r.toilet||0)}" onchange="setToiletV2('${date}','${s.id}',${l.id},this.value)"></label><label class="full">Причина<input value="${esc(r.reason||'')}" onchange="setReasonV2('${date}','${s.id}',${l.id},this.value)"></label></div>${c?'<div class="critical-box">>20 мин — вне очереди</div>':''}</div></td>`}).join('');const note=Object.values(activeGroupData().attendance[date]?.[s.id]?.lessons||{}).map(x=>x.reason).filter(Boolean)[0]||'';return `<tr><td>${i+1}</td><td class="student-name ${crit?'late-critical':''}">${esc(s.name)} ${crit?'<span class="critical-box">🔴</span>':''}</td>${cells}<td>${esc(note)}</td></tr>`}).join('')}
async function setAttendanceV2(date,sid,no,status){const r=attendanceRecord(date,sid,no);r.status=status;if(status==='absent'){r.arrival='';r.late=0}else if(r.arrival)r.late=lateMinutes(LESSONS_V2[no-1],r.arrival);collegeData.history.unshift({id:uid('h'),date,text:`${studentsFor(currentGroupCode()).find(s=>s.id===sid)?.name||'Студент'} — ${LESSONS_V2[no-1].name}: ${status}`});collegeData.history=collegeData.history.slice(0,300);await cloudSave();renderCollegeJournal();renderCollegeHome()}
async function setArrivalV2(date,sid,no,arrival){const r=attendanceRecord(date,sid,no);r.arrival=arrival;r.late=lateMinutes(LESSONS_V2[no-1],arrival);if(arrival)r.status=r.late>0?'late':'present';await cloudSave();renderCollegeJournal();renderCollegeHome()}
async function setToiletV2(date,sid,no,val){attendanceRecord(date,sid,no).toilet=Math.max(0,Number(val)||0);await cloudSave();renderCollegeJournal();}
async function setReasonV2(date,sid,no,val){attendanceRecord(date,sid,no).reason=val;await cloudSave()}
function renderCollegeHome(){const date=dateKey(),students=studentsFor(currentGroupCode()),recs=[];students.forEach(s=>LESSONS_V2.forEach(l=>recs.push(attendanceRecord(date,s.id,l.id,false)||{})));const present=recs.filter(r=>r.status==='present').length,absent=recs.filter(r=>r.status==='absent').length,late=recs.filter(r=>r.status==='late').length,criticalN=recs.filter(r=>Number(r.late)>20).length;const set=(id,v)=>{const e=document.getElementById(id);if(e)e.textContent=v};set('homeDate',new Date().toLocaleDateString('ru-RU'));set('homeStudents',students.length);set('homePresent',present);set('homeAbsent',absent);set('homeLate',late);document.getElementById('homeAlerts').innerHTML=criticalN?`<div class="alert">🔴 Критических опозданий сегодня: <b>${criticalN}</b></div>`:'<div class="success-message">Срочных проблем нет.</div>';document.getElementById('homeDuties').innerHTML=renderDutyNames(date)}
function renderDutyNames(date){const g=activeGroupData();g.duties.daily??={};if(!g.duties.daily[date]){const all=studentsFor(currentGroupCode()),start=(g.duties.currentIndex||0)%all.length;g.duties.daily[date]=[all[start]?.id,all[(start+1)%all.length]?.id]}return g.duties.daily[date].map((id,i)=>{const s=studentsFor(currentGroupCode()).find(x=>x.id===id);return `<div class="duty-person"><div class="duty-name">${i+1}. ${esc(s?.name||'—')}</div><span class="badge">Дежурный</span></div>`}).join('')}
function renderCollegeDuties(){const date=dateKey(),g=activeGroupData();document.getElementById('todayDuties').innerHTML=renderDutyNames(date)+'<button class="btn btn-success" onclick="completeDutyV2()">Дежурство выполнено</button>';const all=studentsFor(currentGroupCode()),idx=g.duties.currentIndex||0;document.getElementById('dutyQueue').innerHTML=all.map((s,i)=>`<div style="padding:7px;border-bottom:1px solid #ddd"><b>${i+1}.</b> ${esc(s.name)} ${i<idx?'<span class="badge">выполнено</span>':''}</div>`).join('');const extras=[];for(const [d,day] of Object.entries(g.attendance)){for(const [sid,x] of Object.entries(day)){if(Object.values(x.lessons||{}).some(r=>Number(r.late)>20))extras.push({date:d,sid})}}document.getElementById('extraDuties').innerHTML=extras.length?extras.slice(-30).map(x=>{const s=studentsFor(currentGroupCode()).find(y=>y.id===x.sid);return `<div class="danger-text">${esc(s?.name||'—')} — опоздание >20 минут (${x.date})</div>`}).join(''):'<div class="empty">Вне очереди пока никого нет.</div>'}
async function completeDutyV2(){const g=activeGroupData();g.duties.currentIndex=(g.duties.currentIndex||0)+2;await cloudSave();renderCollegeDuties();renderCollegeHome();showToast('Дежурство завершено')}
function renderCollegeStudents(){const list=document.getElementById('studentsList');const students=studentsFor(currentGroupCode());list.innerHTML=students.map((s,i)=>`<div class="student-card"><h3>${i+1}. ${esc(s.name)}</h3><div class="small">ID: ${esc(s.id)}</div></div>`).join('')}
function addStudent(){const input=document.getElementById('newStudentName'),name=input.value.trim();if(!name)return showToast('Введите ФИО');const g=activeGroupData();g.students.push({id:uid(currentGroupCode()),name});input.value='';cloudSave();renderCollegeStudents();showToast('Студент добавлен')}
function deleteStudent(id){const g=activeGroupData();g.students=g.students.filter(s=>s.id!==id);cloudSave();renderCollegeStudents()}
function renderCollegeStats(){const code=currentGroupCode(),students=studentsFor(code),rows=[];for(const s of students){const all=[];for(const day of Object.values(collegeData.groups[code].attendance||{})){const x=day[s.id]?.lessons||{};all.push(...Object.values(x))}const p=all.filter(x=>x.status==='present').length,a=all.filter(x=>x.status==='absent').length,l=all.filter(x=>x.status==='late').length,c=all.filter(x=>Number(x.late)>20).length,t=all.reduce((z,x)=>z+Number(x.toilet||0),0);rows.push({s,p,a,l,c,t})}const total=rows.reduce((z,x)=>z+x.p+x.a+x.l,0);document.getElementById('statTotal').textContent=total;document.getElementById('statPresent').textContent=rows.reduce((z,x)=>z+x.p,0);document.getElementById('statAbsent').textContent=rows.reduce((z,x)=>z+x.a,0);document.getElementById('statLate').textContent=rows.reduce((z,x)=>z+x.l,0);document.getElementById('statCritical').textContent=rows.reduce((z,x)=>z+x.c,0);document.getElementById('statToilet').textContent=rows.reduce((z,x)=>z+x.t,0);document.getElementById('statisticsBody').innerHTML=rows.map((x,i)=>`<tr><td>${i+1}</td><td>${esc(x.s.name)}</td><td>${x.p}</td><td>${x.a}</td><td>${x.l}</td><td class="danger-text">${x.c}</td><td>${x.t}</td></tr>`).join('')}
function renderCollegeHistory(){const e=document.getElementById('historyList');e.innerHTML=(collegeData.history||[]).slice(0,100).map(x=>`<div style="padding:8px;border-bottom:1px solid #ddd"><b>${esc(x.date||'')}</b> — ${esc(x.text||'')}</div>`).join('')||'<div class="empty">История пуста.</div>'}
function renderCollegeNotes(){const e=document.getElementById('notesList');e.innerHTML=(activeGroupData().notes||[]).map(n=>`<div class="student-card"><b>${esc(n.title||'Заметка')}</b><p>${esc(n.text||'')}</p><button class="btn btn-danger" onclick="deleteCollegeNote('${n.id}')">Удалить</button></div>`).join('')||'<div class="empty">Заметок пока нет.</div>'}
async function addNote(){const t=document.getElementById('newNoteText').value.trim();if(!t)return;activeGroupData().notes??=[];activeGroupData().notes.unshift({id:uid('note'),title:'Заметка',text:t,date:dateKey()});document.getElementById('newNoteText').value='';await cloudSave();renderCollegeNotes()}
async function deleteCollegeNote(id){activeGroupData().notes=activeGroupData().notes.filter(x=>x.id!==id);await cloudSave();renderCollegeNotes()}
function renderCollegeCalendar(){const grid=document.getElementById('calendarGrid');if(!grid)return;const d=calendarCursor,y=d.getFullYear(),m=d.getMonth();document.getElementById('calendarTitle').textContent=d.toLocaleDateString('ru-RU',{month:'long',year:'numeric'});const first=new Date(y,m,1).getDay()||7,days=new Date(y,m+1,0).getDate();grid.innerHTML=['Пн','Вт','Ср','Чт','Пт','Сб','Вс'].map(x=>`<div class="calendar-head">${x}</div>`).join('');for(let i=1;i<first;i++)grid.innerHTML+='<div></div>';for(let day=1;day<=days;day++){const k=`${y}-${String(m+1).padStart(2,'0')}-${String(day).padStart(2,'0')}`,h=isHoliday(k),ev=collegeData.events.some(e=>String(e.start).slice(0,10)===k);grid.innerHTML+=`<div class="calendar-day ${k===dateKey()?'today':''} ${h||ev?'has-event':''}"><b>${day}</b>${h?`<div class="calendar-event">🎉 ${esc(h.title)}</div>`:''}${ev?'<div class="calendar-event">📢 Событие</div>':''}</div>`}}
function changeMonth(delta){calendarCursor=new Date(calendarCursor.getFullYear(),calendarCursor.getMonth()+Number(delta),1);renderCollegeCalendar()}
function goToday(){calendarCursor=new Date();renderCollegeCalendar()}
function renderReminders(){const e=document.getElementById('remindersList');if(!e)return;e.innerHTML=(collegeData.reminders||[]).map(r=>`<div class="student-card"><b>${esc(r.date)}</b> — ${esc(r.text)}</div>`).join('')}
function addReminder(){const date=document.getElementById('reminderDate').value,text=document.getElementById('reminderText').value.trim();if(!date||!text)return;collegeData.reminders.push({id:uid('rem'),date,text});cloudSave();renderCollegeCalendar();renderReminders()}
function renderSettings(){const a=document.getElementById('settingGroupName'),b=document.getElementById('settingLateLimit');if(a)a.value=groupInfo(currentGroupCode()).name;if(b)b.value=collegeData.lateLimit||20}
async function saveSettings(){collegeData.lateLimit=Number(document.getElementById('settingLateLimit').value)||20;await cloudSave();refreshCollege();showToast('Настройки сохранены')}
function exportData(){const blob=new Blob([JSON.stringify(collegeData,null,2)],{type:'application/json'}),a=document.createElement('a');a.href=URL.createObjectURL(blob);a.download=`journal-${dateKey()}.json`;a.click();URL.revokeObjectURL(a.href)}
function importData(ev){const f=ev.target.files[0];if(!f)return;const r=new FileReader();r.onload=()=>{try{normalizeCollegeData(JSON.parse(r.result));saveLocal();refreshCollege();cloudSave();showToast('Данные импортированы')}catch(e){showToast('Неверный JSON')}};r.readAsText(f)}
function setToday(){const e=document.getElementById('journalDate');if(e){e.value=dateKey();renderCollegeJournal()}}
function refreshCollege(){ensureGroups();document.getElementById('headerGroupLabel').textContent=groupInfo(currentGroupCode()).name;document.getElementById('activeGroupSelect').value=currentGroupCode();fillGroupSelects();renderCollegeHome();renderCollegeJournal();renderCollegeDuties();renderCollegeStudents();renderCollegeStats();renderCollegeHistory();renderCollegeNotes();renderCollegeCalendar();renderReminders();renderCollegeSchedule();renderGrades();renderEvents();renderSettings();renderAuth();applyPermissions()}
function showPage(pageId,button){document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));const p=document.getElementById(pageId);if(p)p.classList.add('active');document.querySelectorAll('nav button').forEach(b=>b.classList.remove('active'));if(button)button.classList.add('active');refreshCollege()}
function showToast(message){const e=document.getElementById('toast');if(!e)return;e.textContent=message;e.style.display='block';clearTimeout(window.__toastTimer);window.__toastTimer=setTimeout(()=>e.style.display='none',2600)}
function applyLegacyCompatibility(){window.loadFromCloud=cloudLoad;window.saveToCloud=cloudSave;window.renderJournal=renderCollegeJournal;window.renderDuties=renderCollegeDuties;window.renderStudents=renderCollegeStudents;window.renderStatistics=renderCollegeStats;window.renderHistory=renderCollegeHistory;window.renderNotes=renderCollegeNotes;window.renderCalendar=renderCollegeCalendar;window.renderSettings=renderSettings;window.addStudent=addStudent;window.addNote=addNote;window.setToday=setToday;}
async function initCollege(){loadLocal();ensureGroups();document.getElementById('journalDate').value=dateKey();document.getElementById('scheduleDate').value=dateKey();document.getElementById('overrideDate').value=dateKey();document.getElementById('holidayDate').value=dateKey();document.getElementById('gradeDate').value=dateKey();document.getElementById('eventStart').value=new Date(Date.now()-new Date().getTimezoneOffset()*60000).toISOString().slice(0,16);applyLegacyCompatibility();refreshCollege();await cloudLoad();await loadAuth();if(collegeSupabase){collegeRealtime=collegeSupabase.channel('college-journal-v2').on('postgres_changes',{event:'*',schema:'public',table:'group3_journal',filter:'id=eq.main'},payload=>{if(payload.new?.data){normalizeCollegeData(payload.new.data);saveLocal();refreshCollege();updateSync('☁️ Облако подключено',true)}}).subscribe(status=>{if(status==='SUBSCRIBED')updateSync('☁️ Облако подключено',true)});collegeSupabase.auth.onAuthStateChange(async(_event,session)=>{collegeSession=session;collegeProfile=null;if(session){const {data}=await collegeSupabase.from('profiles').select('*').eq('id',session.user.id).maybeSingle();collegeProfile=data||null}renderAuth();applyPermissions()})}}
initCollege();
</script>
</body>
</html>
