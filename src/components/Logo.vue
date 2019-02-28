<template>
    <div>
        <div>
            <label for="fontName">Google Font Name</label>
            <input type="text" id="fontName" name="fontName" v-model="fontName" />
            <label for="fontSize">Font Size</label>
            <input type="text" id="fontSize" name="fontSize" v-model="fontSize" />
            <label for="width">Width</label>
            <input type="text" id="width" name="width" v-model="width" />
            <label for="height">Height</label>
            <input type="text" id="height" name="height" v-model="height" />
            <button @click="compose">Go!</button>
        </div>
        <img :src="src" :width="width" :height="height" alt="Logo">
        <span :style="style" id="logo-text">Cylc</span>
    </div>
</template>

<script>
    import WebFontLoader from 'webfontloader';

    function dragElement(elmnt) {
        let pos1 = 0;
        let pos2 = 0;
        let pos3 = 0;
        let pos4 = 0;
        if (document.getElementById(`${elmnt.id}header`)) {
            // if present, the header is where you move the DIV from:
            document.getElementById(`${elmnt.id}header`).onmousedown = dragMouseDown;
        } else {
            // otherwise, move the DIV from anywhere inside the DIV:
            elmnt.onmousedown = dragMouseDown;
        }

        function dragMouseDown(e) {
            e = e || window.event;
            e.preventDefault();
            // get the mouse cursor position at startup:
            pos3 = e.clientX;
            pos4 = e.clientY;
            document.onmouseup = closeDragElement;
            // call a function whenever the cursor moves:
            document.onmousemove = elementDrag;
        }

        function elementDrag(e) {
            e = e || window.event;
            e.preventDefault();
            // calculate the new cursor position:
            pos1 = pos3 - e.clientX;
            pos2 = pos4 - e.clientY;
            pos3 = e.clientX;
            pos4 = e.clientY;
            // set the element's new position:
            elmnt.style.top = `${elmnt.offsetTop - pos2}px`;
            elmnt.style.left = `${elmnt.offsetLeft - pos1}px`;
        }

        function closeDragElement() {
            // stop moving when mouse button is released:
            document.onmouseup = null;
            document.onmousemove = null;
        }
    }

    export default {
        name: 'Logo',

        props: ['src'],

        data: () => {
            return {
                width: 300,
                height: 300,
                fontName: 'Oswald',
                fontSize: '100px',
            };
        },

        computed: {
            style: {
                get() {
                    return 'font-family: ' + this.fontName + '; font-size: ' + this.fontSize;
                },
            },
        },

        created() {
            WebFontLoader.load({
                google: {
                    families: [this.fontName],
                },
            });
        },

        mounted() {
            dragElement(document.getElementById('logo-text'));
        },

        methods: {
            compose(event) {
                WebFontLoader.load({
                    google: {
                        families: [this.fontName],
                    },
                });
                event.preventDefault();
            },
        },

    };
</script>

<style scoped>
    #logo-text {
        position: absolute;
        z-index: 9;
        text-align: center;
    }
</style>
