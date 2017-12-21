<template>
    <div>
        <small style="position:relative; top:-15px;">*
            <slot name="title"></slot>
        </small>


        <div class="mdl-grid">

            <div class="mdl-cell--10-col cellbutbot">

                <button class="mdl-button mdl-js-button mdl-button--raised mdl-js-ripple-effect mdl-button--colored"
                        @click="add_cand(electionType)">
                    <i class="material-icons">add_circle_outline</i>
                    Додати
                    <slot name="type">кандидата</slot>
                </button>
                <button class="mdl-button mdl-js-button mdl-button--raised mdl-js-ripple-effect mdl-button--accent"
                        style="float: right"
                        @click="remove_candidates(electionType.title)">
                    <i class="material-icons">highlight_off</i>
                    Видалити усі записи
                </button>

            </div>

            <div class="mdl-cell--10-col">
                <table class="mdl-data-table mdl-js-data-table mdl-shadow--2dp" width="100%">
                    <thead displaySelectAll="false">
                    <tr>
                        <th class="mdl-data-table__cell--non-numeric">№</th>
                        <th class="mdl-data-table__cell--non-numeric is-full-width">
                            Назва політичної партії</slot>
                        </th>
                        <th class="mdl-data-table__cell--non-numeric center-items"><span
                                class="dont-show-mobile">Видалити</span>
                        </th>
                    </tr>
                    </thead>
                    <tbody displayRowCheckbox="false"
                           v-for="(cand, index) in electionType.dynamic_fields.cand">
                    <tr>
                        <td class="mdl-data-table__cell--non-numeric">{{ index + 1 }}</td>
                        <td class="mdl-data-table__cell--non-numeric">
                            <input class="is-full-width" type="text" v-model="cand.name" @keyup="rewrite_field(cand)"/>
                        </td>

                        <td class="mdl-data-table__cell--non-numeric  center-items">
                            <a class="rem_urn" title="Видалити" @click="rem_cand(electionType, index)"><i
                                    class="material-icons"
                                    style="color:red;">delete</i></a>
                        </td>
                    </tr>
                    </tbody>
                </table>

            </div>

            <div class="mdl-cell--10-col cellbuttop">
                <button class="mdl-button mdl-js-button mdl-button--raised"
                        style="background-color:snow;"
                        @click="load_cand"
                ><i class="material-icons">attach_file</i>
                    Імпорт даних з файлу
                </button>
                <span style="font-size:0.8em; margin-left: 15px">
                    <a @click="qwe"> <i class="material-icons" title="Як це працює?">help_outline</i> </a>
                </span>
                <input type="file" style="display: none">
            </div>


        </div>


    </div>
</template>


<script type="text/javascript">


    var import_info =
        '<div class="modal-dialog">\n' +
        '                <div class="modal-content">\n' +
        '                    <div class="modal-header">\n' +
        '                    </div>\n' +
        '                    <div class="modal-body" style="text-align:justify;">\n' +
        '\n' +
        '                        <p>Якщо у виборах бере участь велика кількість кандидатів і внесення відповідних даних потребує\n' +
        '                            багато часу, ви можете завантажити готовий файл з переліком кандидатів / партій, що беруть\n' +
        '                            участь у виборах. Для цього:</p>\n' +
        '                        <ol>\n' +
        '                            <li>Перевірте наявність готового файлу з переліком кандидатів/партій для цих конкретних\n' +
        '                                виборів на <a href="http://vyborkom.org/kb/article.php?id=1090" target="_blank">сайті\n' +
        '                                    «Виборком»</a>. Якщо такий файл знайдено – скачайте його на свій комп‘ютер, після\n' +
        '                                чого імпортуйте дані за допомогою кнопки «Імпорт даних з файлу».\n' +
        '                            </li>\n' +
        '                            <li>Після імпорту обов‘язково перевірте правильність імпортованих даних на предмет наявності\n' +
        '                                кандидатів, які могли в останній момент зняти свою кандидатуру. Видалити таких\n' +
        '                                кандидатів можна натиснувши «<i class="fa fa-fw fa-remove" style="color:red;"></i>»\n' +
        '                                напроти прізвища відповідного кандидата\n' +
        '                            </li>\n' +
        '                            .\n' +
        '                        </ol>\n' +
        '                        <p>Ви можете власноруч створити файл з переліком кандидатів / партій для відповідних виборів.\n' +
        '                            Для цього:</p>\n' +
        '                        <ol>\n' +
        '                            <li>Отримайте інформацію щодо актуального переліку партій та кандидатів з сайту <a\n' +
        '                                    href="http://cvk.gov.ua/" target="_blank">Центральної виборчої комісії</a>.\n' +
        '                            </li>\n' +
        '                            <li>Створіть текстовий файл в програмі "Блокнот" (або будь-якому іншому текстовому\n' +
        '                                редакторі, що працює з текстами). Вставте перелік партій або кандидатів таким чином\n' +
        '                                кожна назва партії або кожні прізвище, ім\'я та по батькові кандидатів починалися з\n' +
        '                                нового рядка.\n' +
        '                            </li>\n' +
        '                            <li>Збережіть файл з кодуванням UTF-8. Для програми "Блокнот" необхідно обрати Файл -\n' +
        '                                Зберегти як та обрати UTF-8 в опції "Кодування"\n' +
        '                            </li>\n' +
        '                            <li>Назва файлу може бути будь-якою, проте намагайтеся назвати файл так, щоб не загубити\n' +
        '                                його серед інших. Бажано, щоб назва була написана латиницею та не перевищувала 255\n' +
        '                                символів\n' +
        '                            </li>\n' +
        '                            <li>Завантажуйте файл в програму за допомогую кнопки "Імпорт даних з файлу"</li>\n' +
        '                        </ol>\n' +
        '                    </div>\n' +
        '                </div>\n' +
        '            </div>';

    export default {
        props: ['electionType'],
        methods: {
            rewrite_field: function (cand) {
                let name = JSON.parse(JSON.stringify(cand.name));
                name = name.replace(/[ыЫэЭёЁъЪA-Za-z]/gi, "");
                if (this.electionType.title == 'parl_major') {
                    name = name.toLowerCase().split(' ').map(function (word) {
                        return (word.charAt(0).toUpperCase() + word.slice(1));
                    }).join(' ');
                }
                cand.name = name;

            },
            add_cand: function (elect_type) {
                elect_type.dynamic_fields.cand.push({name: ""})
            },
            rem_cand: function (elect_type, ind) {
                elect_type.dynamic_fields.cand.splice(ind, 1)
            },
            remove_candidates: function (elect_type) {
                this.$emit('remove-candidates', elect_type)
            },
            load_cand: function () {
                console.log(this.electionType)
            },
            qwe: function () {
                showDialog({
                    title: "Імпорт даних",
                    text: import_info,
                    positive: {
                        title: 'Закрити',
                        onClick: function (e) { }
                    }
                })
            }

        }
    }
    ;
</script>
