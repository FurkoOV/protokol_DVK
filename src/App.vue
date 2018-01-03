<template>
    <div id="wrapper" class="mdl-layout mdl-js-layout mdl-layout--fixed-drawer mdl-layout--fixed-header">
        <header class="my-header mdl-layout__header mdl-color--grey-100 mdl-color-text--grey-600">
            <div class="mdl-layout__header-row">
                <span class="mdl-layout-title">Перевірка протоколу</span>
                <div class="mdl-layout-spacer"></div>
                <a class="mdl-button" href="javascript:window.close()"><i class="material-icons">input</i> Вихід</a>
            </div>
        </header>
        <div class="my-drawer mdl-layout__drawer mdl-color--blue-grey-900 mdl-color-text--blue-grey-50">

            <header class="my-drawer-header">
                <img src="./assets/gerb_cvk.png" class="logo">
                <div class="m-head" v-if="common.dvk_num.value">ДВК № <strong v-model="common.dvk_num.value">{{
                    common.dvk_num.value }}</strong>
                </div>
            </header>
            <nav class="my-navigation mdl-navigation mdl-color--blue-grey-800">
                <a class="mdl-navigation__link" href="#" @click="switchPage('sett')"><i
                        class="mdl-color-text--blue-grey-700 material-icons"
                        role="presentation">build</i> Налаштування</a>

                <a class="mdl-navigation__link" href="#" @click="switchPage('prot')"><i
                        class="mdl-color-text--blue-grey-700 material-icons"
                        role="presentation">create</i> Формування протоколу</a>
                <a class="mdl-navigation__link" href="#" @click="switchPage('law')"><i
                        class="mdl-color-text--blue-grey-700 material-icons"
                        role="presentation">import_contacts</i> Нормативні акти</a>
                <a class="mdl-navigation__link" href="#" @click="switchPage('about')"><i
                        class="mdl-color-text--blue-grey-700 material-icons"
                        role="presentation">live_help</i> Про програму</a>
                <div class="mdl-layout-spacer"></div>
                <a class="mdl-navigation__link" href="#"><i class="mdl-color-text--blue-grey-400 material-icons"
                                                            role="presentation">help_outline</i><span
                        class="visuallyhidden">Help</span></a>
            </nav>


            <pre style="display: block">{{ $data }}</pre>
        </div>


        <main class="mdl-layout__content mdl-color--grey-100">

            <div class="mdl-color--white mdl-shadow--2dp mdl-cell mdl-cell--12-col mdl-grid">

                <div id="content" class="mdl-layout__content" v-show="component==='sett'">

                    <div class="page-content">

                        <div class="mdl-layout-title">Налаштування даних ДВК
                            <button v-show="common.election_date.value" @click="remove_alldata"
                                    class="mdl-button mdl-js-button mdl-button--raised mdl-js-ripple-effect mdl-button--accent"
                                    style="float: right">Очистити усі дані
                            </button>
                        </div>
                        <br>
                        <hr>
                        <small style="position:relative; top:-15px;">* Для переходу до наступного поля даних
                            користуйтесь мишкою або клавішою Tab
                        </small>
                        <div class="mdl-grid mdl-cell--9-col">
                            <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label mdl-cell--4-col">
                                <input class="mdl-textfield__input datepicker" type="text"
                                       pattern="^(\d{1,2})\.?(\d{1,2})\.?(\d{4})$"
                                       id="election_date" v-model="common.election_date.value">
                                <div id="container"></div>
                                <label class="mdl-textfield__label" for="election_date">Дата виборів</label>
                                <span class="mdl-textfield__error">Невірний формат дати!</span>
                            </div>
                            <div class="mdl-layout-spacer"></div>


                            <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label mdl-cell--4-col">
                                <input class="mdl-textfield__input" type="text" id="dvk_num"
                                       v-model="common.dvk_num.value" pattern="[0-9]{6}" maxlength="6">
                                <label class="mdl-textfield__label" for="dvk_num">№ виборчої дільниці</label>
                                <span class="mdl-textfield__error">Невірний формат!</span>
                            </div>
                            <div class="mdl-layout-spacer"></div>

                            <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label mdl-cell--4-col">
                                <input class="mdl-textfield__input" type="text" id="members_num"
                                       v-model="common.members_num.value" pattern="^[4-9]|[1][0-8]$" maxlength="2">
                                <label class="mdl-textfield__label" for="members_num">Кількість членів ДВК</label>
                                <span class="mdl-textfield__error">Від 4 до 18!</span>
                            </div>


                        </div>

                        <div id="electtype">
                            <div class="mdl-layout__title">Типи виборів, які проводить ДВК</div>
                            <hr>
                            <small style="position:relative; top:-15px;">* Вкажіть, які саме вибори проводяться
                                на
                                вашій
                                дільниці та
                                уточніть їх тип
                            </small>

                            <div class="mdl-grid">
                                <div class="mdl-cell--12-col">
                                    <input type="checkbox" id="check_parl_party" value="parl_party"
                                           v-model="electionTypes">
                                    <label for="check_parl_party">Вибори народних депутатів України за
                                        пропорційною
                                        системою
                                        у
                                        загальнодержавному багатомандатному виборчому окрузі</label><br/>

                                    <input type="checkbox" id="check_parl_major" value="parl_major"
                                           v-model="electionTypes">
                                    <label for="check_parl_major">Вибори народних депутатів України за
                                        мажоритарною
                                        системою відносної більшості в одномандатних виборчих округах</label>
                                    <br/>
                                    <!--                                    <br/>
                                                                        <input type="checkbox" id="check_local_region" value="local_region"
                                                                               v-model="electionTypes">
                                                                        <label for="check_local_region">Вибори депутатів обласної ради (ВР АР
                                                                            Крим)</label><br/>

                                                                        <div style="padding-left:30px;" v-show="elect('local_region')">
                                                                            <input type="radio" id="local_oblast"
                                                                                   value="Верховної Ради Автономної Республіки Крим"
                                                                                   v-model="local_region.type">
                                                                            <label for="local_oblast">Верховної Ради Автономної Республіки
                                                                                Крим</label>
                                                                            <br/>
                                                                            <input type="radio" id="local_ark" value="обласної ради"
                                                                                   v-model="local_region.type">
                                                                            <label for="local_ark">обласної ради</label>
                                                                            <br/>
                                                                        </div>

                                                                        <input type="checkbox" id="check_local_district" value="local_district"
                                                                               v-model="electionTypes">
                                                                        <label for="check_local_district">Вибори депутатів районної (районної в
                                                                            місті)
                                                                            ради</label><br/>

                                                                        <div style="padding-left:30px;" v-show="elect('local_district')">
                                                                            <input type="radio" id="local_single_district" value="районної ради"
                                                                                   v-model="local_district.type">
                                                                            <label for="local_single_district">районної ради</label>
                                                                            <br/>
                                                                            <input type="radio" id="local_district_in_city"
                                                                                   value="районної в місті ради"
                                                                                   v-model="local_district.type">
                                                                            <label for="local_district_in_city">районної в місті ради</label>
                                                                            <br/>
                                                                        </div>

                                                                        <input type="checkbox" id="check_local_city" value="local_city"
                                                                               v-model="electionTypes">
                                                                        <label for="check_local_city">Вибори депутатів міської ради</label><br/>

                                                                        <input type="checkbox" id="check_local_mayor" value="local_mayor"
                                                                               v-model="electionTypes">
                                                                        <label for="check_local_mayor">Вибори міського (селищного, сільського)
                                                                            голови</label><br/>

                                                                        <div style="padding-left:30px;" v-show="elect('local_mayor')">
                                                                            <input type="radio" id="mayor_city" value="міського голови"
                                                                                   v-model="local_mayor.type">
                                                                            <label for="mayor_city">міського голови</label>
                                                                            <br/>
                                                                            <input type="radio" id="mayor_township" value="сільського голови"
                                                                                   v-model="local_mayor.type">
                                                                            <label for="mayor_township">сільського голови</label>
                                                                            <br/>
                                                                            <input type="radio" id="mayor_village" value="селищного голови"
                                                                                   v-model="local_mayor.type">
                                                                            <label for="mayor_village">селищного голови</label>
                                                                            <br/>


                                                                        </div>

                                                                        <input type="checkbox" id="check_local_village" value="local_village"
                                                                               v-model="electionTypes">
                                                                        <label for="check_local_village">Вибори депутатів сільської (селищної)
                                                                            ради</label><br/>

                                                                        <div style="padding-left:30px;" v-show="elect('local_village')">
                                                                            <input type="radio" id="village_village" value="сільської ради"
                                                                                   v-model="local_village.type">
                                                                            <label for="village_village">сільської ради</label>
                                                                            <br>
                                                                            <input type="radio" id="village_township" value="селищної ради"
                                                                                   v-model="local_village.type">
                                                                            <label for="village_township">селищної ради</label>
                                                                            <br>
                                                                        </div>

                                                                        <input type="checkbox" id="check_local_elderman" value="local_elderman"
                                                                               v-model="electionTypes">
                                                                        <label for="check_local_elderman">Вибори старости</label>

                                    -->
                                </div>
                            </div>
                        </div>


                        <div id="distrnum"
                             v-show="electionTypes.length > 0 && electionTypes!='local_elderman' ">
                            <div class="mdl-grid">
                                <div class="mdl-cell--6-col">

                                    <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label ovonum"
                                         v-show="elect('parl_major') || elect('parl_party')">
                                        <input class="mdl-textfield__input" type="text" id="parl_ovk_num"
                                               v-model="common.parl_ovk_num.value" pattern="^[1-9]|[1-9][0-9]{1,2}$"
                                               maxlength="3">
                                        <label class="mdl-textfield__label" for="parl_ovk_num">№ виборчого округу на
                                            виборах народних депутатів України</label>
                                        <span class="mdl-textfield__error">Невірний формат!</span>
                                    </div>

                                    <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label ovonum"
                                         v-show="elect('local_region')">
                                        <input class="mdl-textfield__input" type="text" id="local_region_tvk_num"
                                               v-model="common.local_region_tvk_num.value"
                                               pattern="^[1-9]|[1-9][0-9]{1,2}$">
                                        <label class="mdl-textfield__label" for="local_region_tvk_num">№ виборчого
                                            округу на виборах {{local_region.type}}</label>
                                        <span class="mdl-textfield__error">Невірний формат!</span>
                                    </div>

                                    <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label ovonum"
                                         v-show="elect('local_district')">
                                        <input class="mdl-textfield__input" type="text" id="local_district_tvk_num"
                                               v-model="common.local_district_tvk_num.value"
                                               pattern="^[1-9]|[1-9][0-9]{1,2}$">
                                        <label class="mdl-textfield__label" for="local_district_tvk_num">№ виборчого
                                            округу на виборах {{local_district.type}}</label>
                                        <span class="mdl-textfield__error">Невірний формат!</span>
                                    </div>

                                    <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label ovonum"
                                         v-show="elect('local_city')">
                                        <input class="mdl-textfield__input" type="text" id="local_city_tvk_num"
                                               v-model="common.local_city_tvk_num.value"
                                               pattern="^[1-9]|[1-9][0-9]{1,2}$">
                                        <label class="mdl-textfield__label" for="local_city_tvk_num">№ виборчого округу
                                            на виборах {{local_city.type}}</label>
                                        <span class="mdl-textfield__error">Невірний формат!</span>
                                    </div>

                                    <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label ovonum"
                                         v-show="elect('local_village')">
                                        <input class="mdl-textfield__input" type="text" id="local_village_tvk_num"
                                               v-model="common.local_village_tvk_num.value"
                                               pattern="^[1-9]|[1-9][0-9]{1,2}$">
                                        <label class="mdl-textfield__label" for="local_village_tvk_num">№ виборчого
                                            округу на виборах {{local_village.type}}</label>
                                        <span class="mdl-textfield__error">Невірний формат!</span>
                                    </div>

                                </div>
                            </div>
                        </div>

                        <div id="urns"
                             v-show="common.parl_ovk_num.value && (elect('parl_major') || elect('parl_party'))">

                            <div class="mdl-layout__title">Налаштування виборчих скриньок ДВК</div>
                            <hr>

                            <div class="mdl-grid">
                                <div class="mdl-cell-1-11-col  cellbutbot">
                                    <button class="mdl-button mdl-js-button mdl-button--raised mdl-js-ripple-effect mdl-button--colored"
                                            @click="add_urn('Стаціонарна скринька')">
                                        <i class="material-icons">add_circle_outline</i>
                                        Стаціонарна скринька
                                    </button>
                                    <button class="mdl-button mdl-js-button mdl-button--raised mdl-js-ripple-effect mdl-button--colored"
                                            @click="add_urn('Переносна скринька')">
                                        <i class="material-icons">add_circle_outline</i>
                                        Переносна скринька
                                    </button>
                                </div>
                                <div class="mdl-cell--10-col">

                                    <table class="mdl-data-table mdl-js-data-table mdl-shadow--2dp" width="100%">
                                        <thead displaySelectAll="false">
                                        <tr>
                                            <th class="mdl-data-table__cell--non-numeric"></th>
                                            <th class="mdl-data-table__cell--non-numeric">Тип скриньки</th>
                                            <th class="mdl-data-table__cell--non-numeric">Номер скриньки</th>
                                            <th class="mdl-data-table__cell--non-numeric  center-items"><span
                                                    class="dont-show-mobile">Видалити</span>
                                            </th>
                                        </tr>
                                        </thead>

                                        <tbody displayRowCheckbox="false" v-for="(urn, index) in common.urns">
                                        <tr>
                                            <td class="mdl-data-table__cell--non-numeric">{{ index + 1 }}</td>
                                            <td class="mdl-data-table__cell--non-numeric">{{ urn.type }}</td>
                                            <td class="mdl-data-table__cell--non-numeric">


                                                <input type="text" class="form-control" placeholder=""
                                                       v-model="urn.number" @change="change_urn(index, urn)"
                                                       onkeypress="return event.charCode >= 48 && event.charCode <= 57">

                                            </td>
                                            <td class="mdl-data-table__cell--non-numeric center-items">
                                                <a class="rem_urn" title="Видалити" @click="rem_urn(index)"><i
                                                        class="material-icons"
                                                        style="color:red;">delete</i></a>
                                            </td>
                                        </tr>
                                        </tbody>
                                        <tfoot adjustForCheckbox="false">
                                        <tr>
                                            <td colspan="4" style="text-align: center">

                                            </td>

                                        </tr>
                                        </tfoot>
                                    </table>
                                </div>

                            </div>
                        </div>

                        <br/><br/>
                        <div id="cand_sett" v-show="electionTypes.length>0 && common.parl_ovk_num.value">

                            <div class="mdl-layout-title">Налаштування даних партій/кандидатів</div>
                            <hr>

                            <div v-show="elect('parl_party')" id="part">

                                <cand-sett :election-type="parl_party" v-on:remove-candidates="removeCandidates">
                                    <span slot="title">Перелік політичних партій, які висунули виборчі списки кандидатів у депутати від політичних партій у загальнодержавному багатомандатному окрузі з виборів народних депутатів України</span>
                                    <span slot="table-title">Назва політичної партії</span>
                                    <span slot="type">партію</span>
                                </cand-sett>
                                <br/>


                                <br/>


                            </div>


                            <div v-show="elect('parl_major')" id="cand">
                                Кандидати
                            </div>

                        </div>


                    </div>


                </div>

                <div id="protokol" class="mdl-layout__content" v-show="component==='prot'">
                    protokol_form
                </div>


                <law v-if="component==='law'"></law>
                <about v-if="component==='about'"></about>

            </div>

        </main>


    </div>


</template>

<script>


    import Prot from './components/prot.vue'
    import Law from './components/law.vue'
    import About from './components/about.vue'
    import CandSett from './components/candSett.vue'


    var electionsOrder = ['parl_party', 'parl_major', 'local_region',
        'local_district', 'local_city', 'local_mayor',
        'local_village', 'local_elderman'];


    var old_data = {};

    var data = {
        component: "sett",
        electionTypes: [],
        current_protocol: "",
        common: {
            election_date: {value: ""},
            dvk_num: {value: ""},
            members_num: {value: ""},
            parl_ovk_num: {value: ""},
            local_region_tvk_num: {value: ""},
            local_city_tvk_num: {value: ""},
            local_district_tvk_num: {value: ""},
            local_village_tvk_num: {value: ""},
            urns: [{type: "Стаціонарна скринька", number: ""},
                {type: "Стаціонарна скринька", number: ""}
            ]
        },
        parl_party: {
            title: "parl_party",
            type: "народних депутатів (у загальнодержавному окрузі в межах одномандатного округу)",
            static_fields: [],
            dynamic_fields: {
                urns: [],
                cand: [{name: ""},
                    {name: ""},
                    {name: ""}]
            }
        },
        parl_major: {
            title: "parl_major",
            type: "народних депутатів (одномандатний виборчий округ)",
            static_fields: [],
            dynamic_fields: {
                urns: [],
                cand: [{name: ""},
                    {name: ""},
                    {name: ""}]
            }
        },
        local_region: {
            type: "обласної ради",
            static_fields: [],
            dynamic_fields: {
                cand: [{name: ""},
                    {name: ""},
                    {name: ""}]
            }
        },
        local_district: {
            type: "районної ради",
            static_fields: [],
            dynamic_fields: {
                cand: [{name: ""},
                    {name: ""},
                    {name: ""}]
            }
        },
        local_city: {
            type: "міської ради",
            static_fields: [],
            dynamic_fields: {
                cand: [{name: ""},
                    {name: ""},
                    {name: ""}]
            }
        },
        local_mayor: {
            type: "міського голови",
            static_fields: [],
            dynamic_fields: {
                cand: [{name: ""},
                    {name: ""},
                    {name: ""}]
            }
        },
        local_village: {
            type: "сільської ради",
            static_fields: [],
            dynamic_fields: {
                cand: [{name: ""},
                    {name: ""},
                    {name: ""}]
            }
        },
        local_elderman: {
            type: "сільського старости",
            static_fields: [],
            dynamic_fields: {
                cand: [{name: ""},
                    {name: ""},
                    {name: ""}]
            }
        }
    };


    export default {
        name: 'app',
        data: function () {
            return data;
        },
        components: {
            Prot
            , Law
            , About
            , CandSett
        },

        methods: {
            removeCandidates: function (electType) {
                this[electType].dynamic_fields.cand.splice(0);
                this[electType].dynamic_fields.cand.push({name: ""}, {name: ""}, {name: ""});
            },

            switchPage: function (currPage) {
                this.component = currPage;
            },
            remove_alldata: function () {
                old_data.common = init_common;
                localStorage.clear();
                location.reload();
            }
            ,

            elect: function (electionType) {
                return this.electionTypes.indexOf(electionType) != -1;
            }
            ,

            change_urn: function (i, urn) {
                for (let k = 0; k < this.parl_major.dynamic_fields.urns.length; k++) {
                    if (this.parl_major.dynamic_fields.urns[k].number === urn.number && this.parl_major.dynamic_fields.urns[k] !== this.parl_major.dynamic_fields.urns[i]) {
                        $.notify('Урну з номером ' + urn.number + ' уже заведено!', {position: "bottom right"})
                        urn.number = '';
                    }
                    else {
                        this.parl_major.dynamic_fields.urns[i].type = urn.type;
                        this.parl_party.dynamic_fields.urns[i].type = urn.type;
                        this.parl_major.dynamic_fields.urns[i].number = urn.number;
                        this.parl_party.dynamic_fields.urns[i].number = urn.number;
                    }
                }
            },
            rem_urn: function (i) {
                this.common.urns.splice(i, 1);
                this.parl_party.dynamic_fields.urns.splice(i, 1);
                this.parl_major.dynamic_fields.urns.splice(i, 1);
            }
            ,
            add_urn: function (urntype) {
                this.common.urns.push({type: urntype, number: ""});
                this.parl_major.dynamic_fields.urns.push({bulletin: "", type: urntype, number: ""});
                this.parl_party.dynamic_fields.urns.push({bulletin: "", type: urntype, number: ""});
            }
        },

        mounted: function () {
            for (let i = 0; i < this.common.urns.length; i++) {
                this.parl_major.dynamic_fields.urns.push({
                    bulletin: "",
                    type: this.common.urns[i].type,
                    number: this.common.urns[i].number
                });
                this.parl_party.dynamic_fields.urns.push({
                    bulletin: "",
                    type: this.common.urns[i].type,
                    number: this.common.urns[i].number
                });


            }

            this.$nextTick(function () {
                let dt = this.common.election_date;
                var $input = $('.datepicker').pickadate({
                    format: 'dd.mm.yyyy',
                    container: '#container',
                    firstDay: 1,
                    disable: [1, 2, 3, 4, 5, 6],
                    onClose: function () {
                        if ($(this.$node).val() === '') {
                            $(this.$node).parent().removeClass('is-dirty is-focused');
                        } else {
                            $(this.$node).parent().addClass('is-dirty');
                            dt.value = $(this.$node).val();

                        }
                    }
                })
                var picker = $input.pickadate('picker');
            })
        },

        watch: {
            'electionTypes': {
                handler: function (val, oldVal) {
                    old_data.electionTypes = JSON.stringify(val);
                    localStorage.dvk_protokol = JSON.stringify(old_data);
                },
                deep: true
            },
            'common': {
                handler: function (val, oldVal) {
                    old_data.common = JSON.stringify(val);
                    localStorage.dvk_protokol = JSON.stringify(old_data);
                },
                deep: true
            },
            'parl_major': {
                handler: function (val, oldVal) {
                    old_data.parl_major = JSON.stringify(val.dynamic_fields.cand);
                    old_data.type_parl_major = JSON.stringify(val.type);
                    localStorage.dvk_protokol = JSON.stringify(old_data);
                },
                deep: true
            },
            'parl_party': {
                handler: function (val, oldVal) {
                    old_data.parl_party = JSON.stringify(val.dynamic_fields.cand);
                    old_data.type_parl_party = JSON.stringify(val.type);
                    localStorage.dvk_protokol = JSON.stringify(old_data);
                },
                deep: true
            },
            'local_region': {
                handler: function (val, oldVal) {
                    old_data.local_region = JSON.stringify(val.dynamic_fields.cand);
                    old_data.type_local_region = JSON.stringify(val.type);
                    localStorage.dvk_protokol = JSON.stringify(old_data);
                },
                deep: true
            },
            'local_district': {
                handler: function (val, oldVal) {
                    old_data.local_district = JSON.stringify(val.dynamic_fields.cand);
                    old_data.type_local_district = JSON.stringify(val.type);
                    localStorage.dvk_protokol = JSON.stringify(old_data);
                },
                deep: true
            },
            'local_city': {
                handler: function (val, oldVal) {
                    old_data.local_city = JSON.stringify(val.dynamic_fields.cand);
                    old_data.type_local_city = JSON.stringify(val.type);
                    localStorage.dvk_protokol = JSON.stringify(old_data);
                },
                deep: true
            },
            'local_mayor': {
                handler: function (val, oldVal) {
                    old_data.local_mayor = JSON.stringify(val.dynamic_fields.cand);
                    old_data.type_local_mayor = JSON.stringify(val.type);
                    localStorage.dvk_protokol = JSON.stringify(old_data);
                },
                deep: true
            },
            'local_village': {
                handler: function (val, oldVal) {
                    old_data.local_village = JSON.stringify(val.dynamic_fields.cand);
                    old_data.type_local_village = JSON.stringify(val.type);
                    localStorage.dvk_protokol = JSON.stringify(old_data);
                },
                deep: true
            },
            'local_elderman': {
                handler: function (val, oldVal) {
                    old_data.local_elderman = JSON.stringify(val.dynamic_fields.cand);
                    old_data.type_local_elderman = JSON.stringify(val.type);
                    localStorage.dvk_protokol = JSON.stringify(old_data);
                },
                deep: true
            }

        }

    }


    if (typeof localStorage.dvk_protokol == "undefined"
    ) {
        old_data = {};
    }
    else {
        old_data = JSON.parse(localStorage.dvk_protokol);

        if (typeof old_data.electionTypes != "undefined") data.electionTypes = JSON.parse(old_data.electionTypes);
        if (typeof old_data.common != "undefined") data.common = JSON.parse(old_data.common);


        for (let i in electionsOrder) {
            if (typeof old_data[electionsOrder[i]] != "undefined") {
                data[electionsOrder[i]].dynamic_fields.cand = JSON.parse(old_data[electionsOrder[i]]);
            }
            if (typeof old_data[('type_' + electionsOrder[i])] != "undefined") {
                data[electionsOrder[i]].type = JSON.parse(old_data[('type_' + electionsOrder[i])]);
            }
        }


    }


</script>


