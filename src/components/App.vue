<template>
    <Page class="main-container">
        <ActionBar title="Calendar" />
        <FlexboxLayout class="home-container" flexDirection="column"
            alignContent="space-between" justifyContent="space-between">
            <FlexboxLayout flexDirection="column">
              <FlexboxLayout class="home-panel" justifyContent="space-between"
                alignItems="flex-start">
                <FlexboxLayout class="block1">
                    <Button class="btn-1" text="<<" @tap="goToLeftYear" />
                    <Button class="btn-1" text="<" @tap="goToLeft" />
                </FlexboxLayout>
                <FlexboxLayout justifyContent="space-between">
                    <Label class="panel-text" :text=months[currentMonth] />
                    <Label class="panel-text" text=' ' />
                    <Label class="panel-text" :text=currentYear />
                </FlexboxLayout>
                <FlexboxLayout class="block1">
                    <Button class="btn-1" text=">" @tap="goToRight" />
                    <Button class="btn-1" text=">>" @tap="goToRightYear" />
                </FlexboxLayout>
            </FlexboxLayout>
            <FlexboxLayout class="home-box" flexDirection="column">
                <FlexboxLayout class="weekdays-container"
                    justifyContent="space-between" width="100%">
                    <Label v-for="(weekday, index) in weekdays" :text="weekday" :key="index">
                    </Label>
                </FlexboxLayout>
                <GridLayout class="days-container"
                    columns="*, *, *, *, *, *, *" :rows="rows">
                    <Label class="days" v-for="(day, index) in daysOfMonth"
                        :text="day" :key="index" :row="index / 7"
                        :col="index % 7" width="100%"
                        :style="{ 'background-color': checkToday(day) }">
                    </Label>
                </GridLayout>
              </FlexboxLayout>
            </FlexboxLayout>
            <Button id="btn-today" class="btn-1" text="Сегодня"
                @tap="goToday" />
        </FlexboxLayout>
    </Page>
</template>

<script>
    export default {
        data() {
            return {
                months: [
                    "Январь",
                    "Февраль",
                    "Март",
                    "Апрель",
                    "Май",
                    "Июнь",
                    "Июль",
                    "Август",
                    "Сентябрь",
                    "Октябрь",
                    "Ноябрь",
                    "Декабрь"
                ],
                weekdays: ["пн", "вт", "ср", "чт", "пт", "сб", "вс"],
                today: new Date(),
                currentMonth: new Date().getMonth(),
                currentYear: new Date().getFullYear()
            };
        },
        computed: {
            daysOfMonth() {
                var date = new Date(this.currentYear, this.currentMonth, 1);
                var result = [];
                while (date.getMonth() == this.currentMonth) {
                    result.push(date.getDate());
                    date.setDate(date.getDate() + 1);
                }
                let limit = (new Date(this.currentYear, this.currentMonth).getDay() + 6) % 7;
                for (let i = 1; i <= limit; i++){
                    result.unshift('');
                }
                return result;
            },
            rows() {
                const rows = [];
                for (let i = 0; i < this.daysOfMonth.length / 2; i++) {
                    rows.push("auto");
                }
                return rows.join(",");
            }
        },
        methods: {
            goToLeft() {
                if (this.currentMonth == 0) {
                    this.currentYear = this.currentYear - 1;
                    this.currentMonth = 11;
                } else {
                    this.currentMonth = this.currentMonth - 1;
                }
            },
            goToRight() {
                if (this.currentMonth == 11) {
                    this.currentYear = this.currentYear + 1;
                    this.currentMonth = 0;
                } else {
                    this.currentMonth = this.currentMonth + 1;
                }
            },
            goToLeftYear(){
                this.currentYear = this.currentYear - 1;
            },
            goToRightYear(){
                this.currentYear = this.currentYear + 1;
            },
            goToday() {
                this.currentMonth = this.today.getMonth();
                this.currentYear = this.today.getFullYear();
            },
            checkToday(day) {
                if (
                    day === this.today.getDate() &&
                    this.currentMonth === this.today.getMonth() &&
                    this.currentYear === this.today.getFullYear()
                ) {
                    return "#FFCD28";
                } else {
                    return "white";
                }
            }
        }
    };
</script>

<style scoped lang="scss">
    * {
        margin: 0;
        padding: 0;
    }

    ActionBar {
        background-color: #8800ff;
        color: white;
        font-size: 24px;
    }
    
    .btn-1 {
      font-size: 14px;
      width: 15px;
      margin: 0;
    }

    #btn-today {
        background-color: orange;
        color: white;
        font-size: 22px;
        padding: 15px;
        height: 180px;
    }

    .main-container {
        width: 100%;
    }

    .home-container {
        margin: 20px 60px;
        padding: 20px;
        width: 100%;
    }

    .home-box {
        margin: 10px 10px;
    }

    .panel-text {
        margin-top: 50px;
        font-size: 14px;
    }

    .weekdays-container {
        border-color: black;
        border-bottom-width: 2px;
        font-weight: bold;
        font-size: 24px;
        margin: 10px 35px;
    }

    .days-container {
        font-size: 24px;
    }

    .days {
        text-align: center;
        margin: 50px 0;
        border-radius: 5px;
    }
</style>