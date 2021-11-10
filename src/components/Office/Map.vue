<template>
    <div class="map">
        <h3>Карта офиса</h3>

        <div
            v-if="!isLoading"
            class="map-root"
        >
            <PlanSVG v-if="!planSVGError" ref="plan-svg"/>
            <div v-else>Ошибка отображения карты</div>
            <WorkPlaceSVG v-show="false" ref="wp-svg" />
        </div>
        <div v-else>Loading...</div>
    </div>
</template>

<script>
import tablesData from '../../assets/data/tables.json';
import PlanSVG from '../../assets/images/map.svg';
import WorkPlaceSVG from '../../assets/images/workPlace.svg';
import * as d3 from 'd3';

export default {
    data() {
        return {
            tables: [],
            isLoading: false,
            planSVG: null,
            planSVGError: false,
            planGroup: null,
            workPlaceSVG: null,
        };
    },
    props: {
        legend: {
            type: Array,
            required: true,
        }
    },
    components: {
        PlanSVG,
        WorkPlaceSVG,
    },
    created() {
        this.loadTables();
    },
    mounted() {
        this.isLoading = true;
        this.planSVG = d3.select(this.$refs['plan-svg']);
        this.workPlaceSVG = d3.select(this.$refs['wp-svg']);
        this.planGroup = this.planSVG.select('#plan-group');
        if (this.planGroup.empty()) {
            this.planSVGError = true;
        } else {
            this.drawTables();
        }
        this.isLoading = false;
    },
    computed: {
        actualizedLegend() {
            this.legend.forEach((item) => {
                const tablesInGroup = this.tables.filter((table) => table['group_id'] === item['group_id']).length;
                if (tablesInGroup !== item.counter) {
                    item.counter = tablesInGroup;
                }
            });
            return this.legend;
        },
    },
    methods: {
        drawTables() {
            this.tables.map((table) => {
                this.planGroup.append('g')
                    .attr('id', `place${table._id}-group`)
                    .attr('division-id', `${table.group_id}`)
                    .classed("workplace", true)
                    .style('transform', `translate(${table.x}px, ${table.y}px) rotate(${table.rotate || 0}deg) scale(0.5)`)
                    .html(this.workPlaceSVG.html())
                    .attr('fill', this.actualizedLegend.find((it) => it.group_id === table.group_id)?.color ?? 'transparent');
            });
        },
        loadTables() {
            this.tables = tablesData;
        },
    },
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
