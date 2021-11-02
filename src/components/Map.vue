<template>
    <div class="map">
        <h3>Карта офиса</h3>
        
        <div
            v-if="!isLoading"
            class="map-root"
        >
            <MapSVG ref="svg" />

            <TableSVG v-show="false" ref="table" />

        </div>
        <div v-else>Loading...</div>
    </div>
</template>

<script>
import eventBus from '@/eventBus.js';
import * as d3 from 'd3';
import MapSVG from '@/assets/images/map.svg';
import TableSVG from '@/assets/images/workPlace.svg';
import tables from '@/assets/data/tables.json';
import legend from '@/assets/data/legend.json';

export default {
    components: {
        MapSVG,
        TableSVG
    },
    data() {
        return {
            isLoading: false,
            svg: null,
            svgGroup: null,
            tables: tables,
            tableSVG: null,
            selectedEmployer: null
        };
    },
    methods: {
        drawTables () {
            const svgTableGroup = this.svgGroup.append("g").classed("group-places", true);

            this.tables.map(table => {
                const svgTable = svgTableGroup
                    .append("g")
                    .classed("employer-place", true)
                    .attr("transform", `translate(${table.x}, ${table.y}) scale(0.5)`)
                    .attr("id", table._id)
                    .on("click", () => { 
                        this.selectedEmployer = table._id;
                        eventBus.$emit('select-employer', table._id);
                    });

                svgTable.append("g")
                    .attr("transform", `rotate(${table.rotate || 0})`)
                    .html(this.tableSVG.html())
                    .attr("fill", legend.find((department) => department.group_id === table.group_id)?.color??"transparent");
            });
        },
    },
    mounted () {
        this.svg = d3.select(this.$refs.svg);
        this.svgGroup = this.svg.select("g");
        this.tableSVG = d3.select(this.$refs.table);

        if (this.svgGroup) {
            this.drawTables();
        } else {
            console.log("No root group");
        }
    }
};
</script>

<style scoped>
.map {
    height: 100%;
    width: 100%;
    padding: 24px;
    overflow: hidden;
    box-sizing: border-box;
    display: flex;
    flex-direction: column;
}

.map-root {
    height: 100%;
    width: 100%;
    overflow: hidden;
    box-sizing: border-box;
}

h3 {
    margin-top: 0px;
}

::v-deep svg {
    height: 100%;
    width: 100%;
}

::v-deep .table {
    cursor: pointer;
}
</style>
