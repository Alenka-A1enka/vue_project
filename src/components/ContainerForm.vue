<template>
    <div class="form">
        <MySelect :list="cities" title="Выберите город" @change_id="changeCity" />
        <MySelect :list="departments_filter" title="Выберите цех" @change_id="changeDepartment" />
        <MySelect :list="employees_filter" title="Выберите сотрудника" @change_id="changeEmployee" />
        <MySelect :list="brigades" title="Выберите бригаду" @change_id="changeBrigade" />
        <MySelect :list="shifts" title="Выберите смену" @change_id="changeShift" />
        <MyButton @click="btnClick">Сохранить в Cookie</MyButton>
    </div>
</template>

<script>
import MySelect from './UI/MySelect.vue';
import MyButton from '@/components/UI/MyButton.vue'

export default {
    components: {
        MySelect, 
        MyButton,
    },
    data() {
        return {
            // Переменные для хранения выбранных пользователем значений. 
            city_id: '',
            department_id: '',
            employee_id: '',
            brigade_id: '',
            shift_id: '',

            // Переменные для хранения фильтрованных цехов и сотрудников. 
            departments_filter: [],
            employees_filter: [],

            // Переменные для хранения исходных данных. 
            cities: [
                { id: 1, name: 'Краснодар' },
                { id: 2, name: 'Москва' },
                { id: 3, name: 'Екатеринбург' }
            ],
            departments: [
                { id: 1, city_id: 1, name: 'Цех 1' },
                { id: 2, city_id: 1, name: 'Цех 2' },
                { id: 3, city_id: 2, name: 'Цех 3' },
                { id: 4, city_id: 2, name: 'Цех 4' },
                { id: 5, city_id: 3, name: 'Цех 5' },
                { id: 6, city_id: 3, name: 'Цех 6' },
            ],
            employees: [
                { id: 1, department_id: 1, name: 'Смирнов Евгений' },
                { id: 2, department_id: 1, name: 'Шведов Артем' },
                { id: 3, department_id: 2, name: 'Жидков Данил' },
                { id: 4, department_id: 2, name: 'Коваленко Владислав' },
                { id: 5, department_id: 3, name: 'Иванов Сергей' },
                { id: 6, department_id: 3, name: 'Сидоров Владимир' },
                { id: 7, department_id: 4, name: 'Кравченко Макар' },
                { id: 8, department_id: 4, name: 'Суховеев Денис' },
                { id: 9, department_id: 5, name: 'Макаренко Олег' },
                { id: 10, department_id: 5, name: 'Языков Никита' },
                { id: 11, department_id: 6, name: 'Гладченко Артур' },
                { id: 12, department_id: 6, name: 'Протасов Станислав' },
            ],
            brigades: [
                { id: 1, name: '8:00-20:00' },
                { id: 2, name: '20:00-8:00' },
            ],
            shifts: [
                { id: 1, name: 'Смена 1' },
                { id: 2, name: 'Смена 2' },
                { id: 3, name: 'Смена 3' },
            ]
        };
    },
    methods: {
        // Функции для изменения текущих выбранных значений. 
        changeCity(city_id) {
            this.city_id = city_id;
        },
        changeDepartment(department_id) {
            this.department_id = department_id;
        },
        changeEmployee(employee_id) {
            this.employee_id = employee_id;
        },
        changeBrigade(brigade_id) {
            this.brigade_id = brigade_id;
        },
        changeShift(shift_id) {
            this.shift_id = shift_id;
        },

        // Функции для инициализации фильтрованных списков. 
        fetchDepartments() {
            this.departments_filter = this.departments;
        },
        fetchEmployees() {
            this.employees_filter = this.employees;
        },

        // Сохранение данных в Cookie (сохраняются только добавленные данные),
        // данные, которые не определены удаляются из cookie. 
        btnClick() {
            const names = ['Город', 'Цех', 'Сотрдуник', 'Бригада', 'Смена']
            const ids = [this.city_id, this.department_id, this.employee_id, this.brigade_id, this.shift_id];
            const objects = [this.cities, this.departments, this.employees, this.brigades, this.shifts];
            for(let i = 0; i < ids.length; i++) {
                if(ids[i]) {
                    // Добавление Cookie. 
                    this.saveCookie(
                        names[i], 
                        (objects[i].filter(elem => elem.id == ids[i]))[0].name
                    );
                }
                else {
                    // Удаление cookie, так как переменная не определена. 
                    this.deleteCookie(names[i])
                }
            }
        },
        saveCookie(name, value) {
            document.cookie = name + '=' + value;
        },
        deleteCookie(name) {
            document.cookie = name + '=' + 0 + "; max-age=-1000000";
        },

    },
    mounted() {
        this.fetchDepartments(); // Инициализация отфильтрованных цехов.
        this.fetchEmployees(); // Инициализация отфильтрованных сотрудников.
    },
    watch: {
        city_id(newValue) {
            // При изменении города изменяются id цехов, после чего отбираются сотрудники в departments_filter (ниже).
            this.departments_filter = this.departments.filter(department => department.city_id == newValue);
        },
        departments_filter(newValue) {
            // Переход управления 
            const lst = []; // Хранит id отфильтрованных цехов для фильтрации сотрудников по городу. 
            newValue.map(department => lst.push(department.id));
            this.employees_filter = this.employees.filter(employee => lst.includes(employee.department_id));
        },
        department_id(newValue) {
            // При выборе конкретного цеха меняется набор сотрудников. 
            this.employees_filter = this.employees.filter(employee => employee.department_id == newValue);
        },
    },
    
}
</script>

<style scoped>
    .form {
        padding: 50px;
        background: #ccc;
        position: fixed; top: 50%; left: 50%;
        -webkit-transform: translate(-50%, -50%);
        -ms-transform: translate(-50%, -50%);
        transform: translate(-50%, -50%);
    }
</style>