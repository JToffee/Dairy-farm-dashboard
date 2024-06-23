<template>
<main>
  <div class="today-sales">
    <summary-title
      title="Today's Sales"
      :startDate="startDate"
      :endDate="endDate"
      class = 'title'
    ></summary-title>
    <div class="summary">
      <div class="tiles">
        <p>
          Gross sales<br>
          <span> KES 500,000 </span>
        </p>
        <p>
          Expenses <br>
          <span> KES 500 </span>
        </p>
        <p>
          Receivables<br>
          <span> KES 5000 </span>
        </p>
        <p>
          Cash in hand
          <span> KES 50,000 </span>
        </p>
        
      </div>

      <table>
        <th class="category-th"></th>
        <th>Retail</th>
        <th>Wholesale</th>
        <th>Leche</th>
        <th>KCC</th>
        <th>Total</th>
        <tbody>
          <tr>
            <td class="category">Units sold (Litres)</td>
            <td>{{ kccMilk }}</td>
            <td>{{ prepaidMilk }}</td>
            <td>{{ retailMilk }}</td>
            <td>{{ retailMilk }}</td>
            <td class ="key-value">5000 L</td>

            
          </tr>
          <tr>
            <td class="category">Sale value(KES)</td>
            <td>{{ prepaidAmount }}</td>
            <td>{{ kccAmount }}</td>
            <td>{{ retailAmount }}</td>
            <td>{{ retailAmount }}</td>
            <td class = "key-value">KES 500,000</td>
          </tr>
          <tr>
            <td class="category">Difference(KES)</td>
            <td>{{ retailDiff }}</td>
            <td>{{ kccDiff }}</td>
            <td>{{ prepaidDiff }}</td>
            <td>{{ prepaidDiff }}</td>
            <td class = "key-value">KES {{ retailDiff }}</td>



          </tr>
        </tbody>
      </table>
      
     
    </div>
  </div>

  <div class="last-week">
    <summary-title
      title="Last week Summary"
      :startDate="startDate"
      :endDate="endDate"
      class = 'title'
    ></summary-title>

    <div class="tiles">
      
      <p>
        Gross sales<br>
        <span> KES 500,000 </span>
      </p>
      <p>
        Profit<br>
        <span> KES 50,000</span>
      </p>
      <p>
        Avg Unit Price<br>
        <span> KES 50/L </span>
      </p>
      <p>
        Receivables<br>
        <span> KES 50,000</span>
      </p>
      <p>
        Cash in hand<br>
        <span> KES 50,000</span>
      </p>
      <p>
        Feeds cost <br>
        <span> KES 50,000 </span>
      </p>
      <p>
        Non-feeds cost<br>
        <span> KES 5000 </span>
      </p>
      <p>
        Feed cost/cwt <br>
        <span>54</span>
      </p>
      <p class="key-value">
        Profit margin <br>
        <span> 10% </span>
      </p>
    </div>

    <div class="chart">
        <img :src="line" alt="graph">
    </div>
  </div>

  <div class="overall">
    <summary-title
      title="Overall Overview"
      startDate=""
      endDate=""
      class = 'title'
    ></summary-title>

    <form action="">
        <div class="form-group">
          <label for="startDate">From</label>
          <input type="date" placeholder="start date"/>
        </div>
        <div class="form-group">
          <label for="endDate">To</label>
          <input type="date"/>
        </div>
    </form>

    
  </div>
</main>
</template>

<script>
  import { sum, sumSale } from "../../util/helpers.js";
  import { TOMILLISECS } from "../../util/config.js";
  // import {sumSale} from '../../util/helpers.js'
  import SummaryTitle from "../ui/summary/SummaryTitle.vue";
  import line from "../../assets/line-chart.png"

  export default {
    name: "SalesSummary",
    components: { SummaryTitle },
    props: { period: Number },
    data() {
      return {
        sales: this.$store.state.sales.sales,
        data: [],
        line: line
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

main {
  display: flex;
  flex-wrap: wrap;
  overflow: hidden;
  flex-basis: 100%;
  gap: 1rem;
  

  .today-sales{
    flex-basis: 100%;
    display: flex;
    flex-wrap: wrap;
    align-items: flex-start;
    padding: 1rem;
    font-size: 1rem;
    height: fit-content;
    background-color: var(--background-grey);
    padding-top: 0;
    padding-bottom: 1.5rem;
  }

  .last-week{
    display: flex;
    flex-wrap: wrap;
    padding: 1rem;
    padding-top: 0;
    background-color: var(--background-grey);
    justify-content: space-evenly;
  }

  .overall{
    flex-basis: 100%;
    display: flex;
    flex-wrap: wrap;
    align-items: flex-start;
    background-color: var(--background-grey);
    padding: 1rem;
    padding-top: 0;
  }

}

.today-sales  {

  .title{
    margin: 0;
  }

  .summary {
    display: flex;
    flex-wrap: wrap;
    margin-top: 0;
    margin-top: 2vh;
    
    table{ 
      margin-top: 0;
      background-color: var(--background-grey);

      th,td {
        padding: 0.5rem;
        text-align: left;
        border-right: 1px solid var(--primary600);
        border-bottom: 1px solid var(--primary600);
      }
      td {
        color: var(--dark-grey);
        
      }
      th {
        font-size: 0.8rem;
        border: none;
        background-color: var(--primary600);
        color: var(--light-grey);
        
      
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

      .category {
        font-weight: bold;
        background-color: transparent;
        color: var(--dark-grey);
        text-align: left;
        font-size: 0.8rem;
        padding-right: 0.5rem;
        border-left: 1px solid var(--primary600);
      }

      .category-th{
          border: none;
          border-bottom: 1px solid var(--primary600);

        }
      
    }

    
  }

}


.last-week{

  .tiles{
    margin-top: 10vh;
    flex-basis: 40%;
    height: fit-content;
    
    p{
    background-color: transparent;
    color: var(--dark-grey);
    border-right: 1px solid var(--mid-grey);
    border-bottom: 1px solid var(--mid-grey);
    margin: 0;
    }
    .key-value{
      color: var(--accent400);
    }
  }

  .chart {
    flex-basis: 55%;

    img{
      width: 500px;
    }
  }
}
  
.tiles{
      display: flex;
      flex-wrap: wrap;
      flex-basis: 30%;
      margin: 0;

      & p{
          margin: 0.1rem;
          width: 8rem;
          text-align: center;
          padding: 0.5rem;
          background-color: var(--primary600);
          color: var(--font-grey);
          font-size: 0.9rem;

          & span {
          margin-top: 2vh;
          font-weight: 600;
          line-height: 8vh;
          font-size: 1rem;
          }
        }
        .key-value{
          color: var(--accent400);
         }
    } 

form{
  flex-basis: 40%;
  display: flex;
  flex-wrap: nowrap;
  padding: 0;
  margin-left: 3vw;
  margin-bottom: 3vh;
  margin-top: 3vh;
  background-color: transparent;
  border: none;
  justify-content: flex-start;

  .form-group{
    display: flex;
    flex-wrap: nowrap;
    padding: 0;
    margin: 0;
    margin-right: 3vw;
  
    label{
      width: fit-content;
      font-size: 0.88rem;
      line-height: 8vh;
      margin-right: 0.5vw;
      padding: 0;
    }
    input{
      width: fit-content;
      min-width: 10vw;
      text-align: center;
      font-size: 0.9rem;
      outline: none;
      border: 1px solid var(--light-grey);
      text-indent: 0;
      padding: 0.1rem 0.6rem;
      

    }
  }
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


.title {
    flex-basis: 100%;
    padding-left: 0; 
    height: fit-content;
}

h4{
  margin-bottom: 1vh;
  flex-basis: 100%;
}

table tr .key-value{
  background-color: var(--accent1200);
  color: var(--dark-grey);
  border: none;
}
</style>
