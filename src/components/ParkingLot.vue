<template>
    <div class="parking">
        <div class="parking-lot-form">
            <form class="parking-lot-form-row" v-on:submit="park">
                <label for="registration-number">Registration Number </label>
                <input type="text" id="registration-number" v-model="registrationNumber"/>
                <br>
                <label for="color">Color</label>
                <input type="color" id="color" v-model="color" />
                <br>
                <input type="submit" value="Park" />
            </form>
            <form class="parking-lot-form-row" v-on:submit="leave">
                <label for="slot-number">Slot Number </label>
                <input type="number" id="slot-number" v-model="slotNumber"/> 
                <input type="submit" value="Leave" />
            </form>
            <form class="parking-lot-form-row" v-on:submit="find">
                <label for="registration-number">Registration Number </label>
                <input type="text" id="registration-number" v-model="registrationNumberOfCarToFind"/> 
                <input type="submit" value="Find" />
            </form>
        </div>
        <div class="parking-lot">
            <div 
                v-bind:key="i" 
                v-for="(slot, i) in slots" 
                v-bind:class="['slot', !slot.isAvailable && slot.car.hightlight ? 'highlighted-slot' : 'not-highlighted-slot']"
            >
                <div class="slot-number">{{ i + 1 }}</div>
                <p v-if="!slot.isAvailable" v-bind:style="{border: '5px solid ' + slot.car.color}">{{ slot.car.registrationNumber }}</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        size: String
    },
    data() {
        return {
            slots: [],
            color: "#000000",
            registrationNumber: "",
            slotNumber: "",
            registrationNumberOfCarToFind: ""
        }
    },
    methods: {
        park(e) {
            e.preventDefault();
            const registrationNumber = this.registrationNumber.toUpperCase();
            const color = this.color;
            let firstAvailableSlot = -1;
            this.slots.some((slot, index) => {
                if(slot.isAvailable) {
                    firstAvailableSlot = index;
                    return true;
                };
            })
            this.slots = this.slots.map((slot, index) => {
                return index === firstAvailableSlot ? { isAvailable: false, car: { registrationNumber, color, hightlight: false } } : slot
            })
        },
        leave(e) {
            e.preventDefault();
            const slotNumber = parseInt(this.slotNumber)
            this.slots = this.slots.map((slot, index) => index === slotNumber - 1 ? { isAvailable: true, car: null } : slot)
        },
        find(e) {
            e.preventDefault();
            const registrationNumber = this.registrationNumberOfCarToFind.toUpperCase();
            let slotFound = false;
            this.slots = this.slots.map((slot) => {
                if(!slot.isAvailable && slot.car.registrationNumber === registrationNumber) {
                    slotFound = true;
                    return { ...slot , car: { ...slot.car, hightlight: true } } 
                }
                return slot;
            });
            if(!slotFound) alert("Car not found")
        }
    },
    created() {
        new Array(parseInt(this.size)).fill().map(() => this.slots.push({ isAvailable: true, car: null }))
    }
}
</script>

<style scoped>
    .parking-lot {
        border: 2px solid cornflowerblue;
        display: flex;
        margin: 50px;
        justify-content: space-evenly;
        align-content: space-between;
        padding: 20px;
        flex-wrap: wrap;
        background-color: #f6f6f6;
    }
    .highlighted-slot {
        border: 1px solid red;
    }
    .not-highlighted-slot {
        border: 1px solid cornflowerblue;
    }
    .slot {
        width: 150px;
        height: 200px;
        margin: 5px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        flex-direction: column;
        padding: 10px;
    }
    .parking-lot-form {
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    .parking-lot-form-row {
        margin: 20px;
    }
    .slot-number {
        width: 25px;
        height: 25px;
        background-color: cornflowerblue;
        border-radius: 12.5px;
        display: flex;
        align-items: center;
        justify-content: center;
        color: white;
    }
</style>