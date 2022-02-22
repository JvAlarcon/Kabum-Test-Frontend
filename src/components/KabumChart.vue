<template>
    <div>
        <form action="#" @submit.prevent="createCategoryData">
            <div class="form-group">
                <select>
                    <option disabled value="">Choose a merchandise category</option>
                    <option v-for="category in categoriesNames" :value="category.value" :key="category.id">
                        {{category.value}}
                    </option>  
                </select>  
            </div>  
        </form> 
    </div>
    <div>
        <svg>
            
        </svg>
    </div>
</template>

<script>
import * as d3 from "d3";
import _ from "lodash";

export default {
    name: "KabumChart",
    props: ["ecommerceSalesData"],
    data() {
        return {
            chart: null,
            ecommerceSalesTotalData: this.ecommerceSalesData,
            categoriesNames: [],
            categoryData: []
        };
    },
    created() {
        this.createCategoriesNames();
    },
    watch: {
        ecommerceSalesData(salesData) {
            if (this.chart != null) {
                this.chart.remove();
            }

            this.renderChart(salesData);
        }
    },
    methods: {
        createCategoriesNames() {
            let salesCategoriesNames = [];
            for(let iterator = 0; iterator < this.ecommerceSalesTotalData.length; iterator++)
            {
                console.log("category name: " + this.ecommerceSalesTotalData[iterator].name);
                let categoryObj = {
                    id: iterator,
                    name: this.ecommerceSalesTotalData[iterator].name
                }

                salesCategoriesNames.push(categoryObj);
            }
            console.log(salesCategoriesNames);
            this.categoriesNames = salesCategoriesNames;      
        },
        createCategoryData(categoryChoosed) {
            let salesCategoryData = [];
            let salesPerYear = [];
            for(let iterator = 0; iterator < this.ecommerceSalesTotalData.length; iterator++) {
                let ecommerceSalesCategoryName = this.ecommerceSalesTotalData[iterator].name;
                let localeCompareResult = categoryChoosed.localeCompare(ecommerceSalesCategoryName);

                if(localeCompareResult === 0)
                {
                    salesPerYear = this.ecommerceSalesTotalData[iterator].salesPerYear;
                    break;
                }
            }

            if(salesPerYear.length !== 0) {
                salesPerYear.forEach(salePerYear => {
                    let SalesCategoryDataObj = {
                        year: salePerYear.year,
                        total: salePerYear.total,
                        ecommerce: salePerYear.ecommerce
                    }

                    salesCategoryData.push(SalesCategoryDataObj);
                });
            }
        },
        renderChart() {
            const margin = 60;
            const svg_width = 1000;
            const svg_height = 600;
            const chart_width = 1000 - 2 * margin;
            const chart_height = 600 - 2 * margin;

            const svg = d3.select("svg")
            .attr("width", svg_width)
            .attr("height", svg_height);

            this.chart = svg.append("g")
            .attr("transform", `translate(${margin}, ${margin})`);

            const ecommerceCategoryData = this.categoryData;

            const yScale = d3.scaleLinear()
            .range([chart_height, 0])
            .domain([0, _.maxBy(ecommerceCategoryData.total, "total").ecommerceSalesData]);

            this.chart.append("g")
            .call(d3.axisLeft(yScale).ticks(_.maxBy(ecommerceCategoryData.total, "total").ecommerceSalesData));

            const xScale = d3.scaleBand()
            .range([0, chart_width])
            .domain(ecommerceCategoryData.map(categoryData => categoryData.year))
            .padding(0.2);

            this.chart.append("g")
            .attr("transform", `translate(0, ${chart_height})`)
            .call(d3.axisBottom(xScale));
        }
    }
    /*,
    created(){
        this.colourScale = d3
        .scaleOrdinal()
        .range(["#5EAFC6", "#FE9922", "93C464", "75739F"])
    },
    methods: {
        packChart(){
            const packChart = d3.pack();
            packChart.size([500, 500]);
            packChart.padding(10);
            const output = packChart(this.packData).descendants();
            return output.map((d, i) => {
                const fill = this.colourScale(d.depth);
                return {
                    id: i + 1,
                    r: d.r,
                    x: d.x,
                    y: d.y,
                    fill,
                    stroke: "grey"
                }
            });
        }
    },
    computed: {
        packData() {
            const merchandiseLines = d3.nest()
            .key(merchandiseLine => merchandiseLine.name)
            .entries(this.ecommerceSalesData);

            const packableMerchandiseLines = {id: "Total Sales", value: merchandiseLines}
            return d3.hierarchy(packableMerchandiseLines, d)
        },
        output(){
            return this.packChart();
        }
    }*/
}
</script>

<style>
</style>