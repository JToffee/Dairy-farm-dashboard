<template>
<main>
  <div class="today-sales">
    <summary-title
      title="Today's Summary"
      :startDate="startDate"
      :endDate="endDate"
      class = 'title'
    ></summary-title>
    <div class="summary">
      <table>
        <th class="category-th"></th>
        <th>Retail</th>
        <th>Wholesale</th>
        <th>Leche</th>
        <th>KCC</th>
        <th>Total</th>
        <tbody>
          <tr>
            <td class="category">Units sold (L)</td>
            <td>{{ kccMilk }}</td>
            <td>{{ prepaidMilk }}</td>
            <td>{{ retailMilk }}</td>
            <td>{{ retailMilk }}</td>
            <td>5000</td>

            
          </tr>
          <tr>
            <td class="category">Sale value(KES)</td>
            <td>{{ prepaidAmount }}</td>
            <td>{{ kccAmount }}</td>
            <td>{{ retailAmount }}</td>
            <td>{{ retailAmount }}</td>
            <td>500,000</td>
          </tr>
          <tr>
            <td class="category">Difference(KES)</td>
            <td>{{ retailDiff }}</td>
            <td>{{ kccDiff }}</td>
            <td>{{ prepaidDiff }}</td>
            <td>{{ prepaidDiff }}</td>
            <td>{{ retailDiff }}</td>



          </tr>
        </tbody>
      </table>
    </div>
  </div>
  <div class="main">

  </div>
</main>
</template>

<script>
  import { sum, sumSale } from "../../util/helpers.js";
  import { TOMILLISECS } from "../../util/config.js";
  // import {sumSale} from '../../util/helpers.js'
  import SummaryTitle from "../ui/summary/SummaryTitle.vue";

  export default {
    name: "SalesSummary",
    components: { SummaryTitle },
    props: { period: Number },
    data() {
      return {
        sales: this.$store.state.sales.sales,
        data: [],
      };
    },
    computed: {
      days() {
        return this.period;
      },
      startDate() {
        return this.$store.state.produce.startDate;
      },
      endDate() {
        return this.$store.state.produce.endDate;
      },
      allMilk() {
        return Intl.NumberFormat("en-GB").format(sum(this.durationData()));
      },
      netMilk() {
        const milk =
          sum(this.durationData()) - sum(this.durationCatData("unsold"));

        return Intl.NumberFormat("en-GB").format(milk);
      },

      totalAmount() {
        return Intl.NumberFormat("en-GB").format(sumSale(this.durationData()));
      },
      netAmount() {
        const amount =
          sumSale(this.durationData()) -
          sumSale(this.durationCatData("unsold"));

        return Intl.NumberFormat("en-GB").format(amount);
      },
      netDiff() {
        return 5;
      },
      amountDiff() {
        return this.differenceOfSum();
      },
      // netAmountDiff() {
      // 	return 5;
      // },
      kccMilk() {
        return Intl.NumberFormat("en-GB").format(
          sum(this.durationCatData("KCC"))
        );
      },
      kccAmount() {
        return Intl.NumberFormat("en-GB").format(
          sumSale(this.durationCatData("KCC"))
        );
      },
      kccDiff() {
        return this.differenceByCat("KCC");
      },

      retailMilk() {
        return Intl.NumberFormat("en-GB").format(
          sum(this.durationCatData("retail"))
        );
      },
      retailAmount() {
        return Intl.NumberFormat("en-GB").format(
          sumSale(this.durationCatData("retail"))
        );
      },
      retailDiff() {
        return this.differenceByCat("retail");
      },
      wholesaleMilk() {
        return Intl.NumberFormat("en-GB").format(
          sum(this.durationCatData("wholesale"))
        );
      },
      wholesaleAmount() {
        return Intl.NumberFormat("en-GB").format(
          sumSale(this.durationCatData("wholesale"))
        );
      },
      wholesaleDiff() {
        return this.differenceByCat("wholesale");
      },
      prepaidMilk() {
        return Intl.NumberFormat("en-GB").format(
          sum(this.durationCatData("prepaid"))
        );
      },
      prepaidAmount() {
        return Intl.NumberFormat("en-GB").format(
          sumSale(this.durationCatData("prepaid"))
        );
      },
      prepaidDiff() {
        return this.differenceByCat("prepaid");
      },
      billedMilk() {
        return Intl.NumberFormat("en-GB").format(
          sum(this.durationCatData("billed"))
        );
      },
      billedAmount() {
        return Intl.NumberFormat("en-GB").format(
          sumSale(this.durationCatData("billed"))
        );
      },
      billedDiff() {
        return this.differenceByCat("billed");
      },
      unsoldMilk() {
        return Intl.NumberFormat("en-GB").format(
          sum(this.durationCatData("unsold"))
        );
      },
      unsoldAmount() {
        return Intl.NumberFormat("en-GB").format(
          sumSale(this.durationCatData("unsold"))
        );
      },
      unsoldDiff() {
        return this.differenceByCat("unsold");
      },
    },
    updated() {
      this.sales = this.$store.state.sales.sales;
      this.days = this.period;
    },
    methods: {
      //set Start date
      setStartDate() {
        return new Date(new Date().setHours(0, 0, 0, 0)).getTime();
      },
      //Filter sales by dates
      durationData(data) {
        this.data = data || this.sales;

        const newData =
          this.data &&
          this.data.filter(
            (item) => item.date <= this.endDate && item.date >= this.startDate
          );

        return newData;
      },
      // specific date data
      dateCatData(category, data, date) {
        return (
          data &&
          data.filter(
            (item) => item.category === category && item.date === date
          )
        );
      },
      //filter by category helper
      getDurationCatData(category, data, startDate, endDate) {
        // if(startDate === endDate)
        return (
          data &&
          data.filter((item) => {
            return (
              item.category === category &&
              item.date <= endDate &&
              item.date >= startDate
            );
          })
        );
      },
      // filter sales by dates and category

      durationCatData(category = null, data) {
        let cat = category;
        this.data = data || this.sales;

        // if (!this.startDate) this.startDate = this.setStartDate();
        // if (!this.endDate) this.endDate = new Date().getTime();

        const newData = this.getDurationCatData(
          cat,
          this.data,
          this.startDate,
          this.endDate
        );

        return newData;
      },

      differenceByCat(category = null, data) {
        let prevDate;
        let prevData;
        let currData;
        this.data = data || this.sales;
        let cat = category;

        // todays value - yesterdays value
        //days = 0
        if (this.days === 0) {
          //previous data
          console.log(this.days);
          prevDate =
            new Date(new Date().setHours(0, 0, 0, 0)).getTime() -
            1 * TOMILLISECS;

          console.log(
            new Date(
              new Date(new Date().setHours(0, 0, 0, 0)).getTime() -
                1 * TOMILLISECS
            ).toLocaleDateString()
          );
          prevData = sumSale(this.dateCatData(cat, this.data, prevDate));

          console.log(prevData);

          currData = sumSale(
            this.dateCatData(cat, this.data, this.setStartDate())
          );
          console.log(currData);
        } else {
          prevDate = this.startDate - this.days * TOMILLISECS;
          console.log(this.days);
          currData = sumSale(
            this.getDurationCatData(
              cat,
              this.data,
              this.startDate,
              this.setStartDate()
            )
          );

          prevData = sumSale(
            this.getDurationCatData(cat, this.data, prevDate, this.startDate)
          );
        }

        let difference = currData - prevData;

        if (difference > 0) return "+ " + difference;
        if (difference < 0) return "- " + Math.abs(difference);

        return difference;
      },

      //COMPARE
      differenceOfSum() {
        let prevDate;
        let prevData;
        let currData;

        // todays value - yesterdays value
        //days = 0
        if (this.days === 0) {
          //previous data

          prevDate =
            new Date(new Date().setHours(0, 0, 0, 0)).getTime() -
            1 * TOMILLISECS;

          prevData = sumSale(
            this.data.filter((item) => item.date === prevDate)
          );

          currData = sumSale(
            this.data.filter((item) => item.date === this.setStartDate())
          );
          console.log(currData, prevData);
        } else {
          currData = sumSale(this.durationData());

          prevDate = this.startDate - this.days * TOMILLISECS;

          prevData = sumSale(
            this.data.filter(
              (item) => item.date <= this.startDate && item.date >= prevDate
            )
          );
          console.log(currData, prevData);
        }

        let difference = currData - prevData;

        if (difference > 0) return "+ " + difference;
        if (difference < 0) return "- " + Math.abs(difference);

        return difference;
      },
    },
    mounted() {},
  };
</script>
<style lang="scss" scoped>
  
.title {
    flex-basis: 100%;
    padding-left: 0;
    
}

h4{
  margin-bottom: 1vh;
  flex-basis: 100%;
}

table tr .key-value{
  background-color: var(--accent1200);
  
}

main {
  display: flex;
  flex-wrap: wrap;
  overflow: hidden;

  .today-sales{
    display: flex;
    flex-wrap: wrap;
    align-items: flex-start;
    padding: 0;
    background-color: blue;

    .summary{
      background-color: aqua;
      margin: 0;
    }
  }



  .title{
    background-color: red;
    margin: 0;
  }
  

}

.today-sales  {
  background-color: var(--background-grey);
  border-top: 1px solid var(--light-grey);
  padding: 0.2rem;
  font-size: 1rem;
  flex-basis: 80%;
  margin: 0;


  & td{
    padding-right: 1rem;
  }

  & th{
    padding-right: 1rem;
    padding-bottom: 1rem;
    color: var(--primary600);
  }

  & tr{
  color: var(--dark-grey);

  }

  & .row-category{
    font-weight: 500;
  }

  & .last-row{
    & td{
      border-top: 1px solid var(--mid-grey);
      padding-top: 0.3rem;
    }
  }

}

table{

  th,td {
    padding: 0.4rem;
    text-align: left;
    border: 1px solid var(--light-grey);
    margin: 0;
  }
  td {
    color: var(--dark-grey);

  }
  th {
    color: var(--font-grey);
    font-size: 0.8rem;
  }
}
  
  .category {
    font-weight: bold;
    /* color: var(--primary600); */
    color: var(--black);
    text-align: left;
    text-indent: 2vw;
  }
  .category-th {
    text-align: left;
    text-indent: 2vw;
  }
  tbody .net {
    background-color: var(--primary700);
    font-weight: bold;
  }
  .net .category {
    color: var(--color-background);
  }
  .net td {
    color: var(--color-background);
  }
</style>
