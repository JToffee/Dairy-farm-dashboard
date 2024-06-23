<template>
<main>

  <div class="today">
    <summary-title
    class="title"
      title="Today's summary"
      :startDate="this.today"
      :endDate="this.today"
    ></summary-title>
    
    <div class="thumbnails-container">

      <thumbnail-vue
        title="Milk Produce"
        :value="produce"
        unit="LITRES"
        :difference="prodDiff()"
      ></thumbnail-vue>

      <thumbnail-vue
        title="Revenue"
        :value="sales"
        unit="KES"
        :difference="salesDiff()"
      ></thumbnail-vue>

      <thumbnail-vue
        title="Expenses"
        value="30000"
        unit="KES"
        difference="- 4500"
      ></thumbnail-vue>
      
      <thumbnail-vue
        title="Net Income"
        value="30000"
        unit="KES"
        difference="+30000"
        valueClass="thumbnail-active"

      ></thumbnail-vue>

    </div>

    <div class="today-averages">
    
      <p>
       Lactation/Cow<br>
        <span> 43.5 KG</span>
      </p>
      <P>
        Price/Litre<br>
        <span>KES 50 </span>
      </P>
      
    </div>
  </div>

  <div class="week-summary">
    <summary-title
    class="title"
      title="Last Week's Summary"
      :startDate="this.today"
      :endDate="this.today"
    ></summary-title>

    <table>
      <th></th>
      <th>Mon</th>
      <th>Tue</th>
      <th>Wed</th>
      <th>Thu</th>
      <th>Fri</th>
      <th>Sat</th>
      <th>Sun</th>
      <th>Total</th>
      <tbody>
          <tr style="">
            <td class="row-category">
              Produce
            </td>
            <td>
              200
            </td>
            <td>
              200
            </td>
            <td>
              200
            </td>
            <td>
              200
            </td>
            <td>
              200
            </td>
            <td>
              200
            </td>
            <td>
              200
            </td>
            <td>
              15000
            </td>
            
          </tr>
          <tr>
            <td  class="row-category">
              Revenue
            </td>
            <td>
              
            </td>
            <td>
              
            </td>
            <td>
              
            </td>
            <td>

            </td>
            <td>
              
            </td>
            <td>
              
            </td>
            <td>
              
            </td>
            
          </tr>
          <tr>
            <td class="row-category" style="padding-bottom: 0.8rem;">
              Expenses
            </td>
            <td>
              
            </td>
            <td>
              
            </td>
            <td>
              
            </td>
            <td>

            </td>
            <td>
              
            </td>
            <td>
              
            </td>
            <td>
              
            </td>
            
          </tr>
          <tr class="last-row">
            <td class="row-category">
              Net Income
            </td>
            <td>
              
            </td>
            <td>
              
            </td>
            <td>
              
            </td>
            <td>

            </td>
            <td>
              
            </td>
            <td>
              
            </td>
            <td>
              
            </td>
            <td>
              
            </td>
          </tr>
      
      </tbody>
      
    </table>

    <div class="pie">
      <h4>Income distribution</h4>
      <img :src=  "pie" alt="pie">
     </div>
  </div>

  <div class="averages">
    <summary-title
    class="title"
      title="Overall averages"
    ></summary-title>

    <div class="farm-averages">
      <h4>Farm averages</h4>
      <p>
      Lactation/Cow<br>
      <span>45.3L/d</span>
      </p>
      <p>
        Litres/Day<br>
        <span>500L</span>
      </p>
        <p>
          Conception rate<br>
          <span>90%</span>
        </p>
        <p>
          Lactating DMI<br>
          <span>500 KG</span>
      </p>
        <p>
          1st calving age <br>
          <span>500L</span>
      </p>
    </div>
    
    <div class="biz-averages">
      <h4>Business averages</h4>
      <P>
        Price/Litre<br>
        <span>KES 50 </span>
      </P>
      <p>
       Income/day<br>
        <span>KES 50,000</span>
      </p>
      <p>
        Expenses/day<br>
        <span>KES 30,000</span>
      </p>
      <P>
        MFPR<br>
        <span> 1.5</span>
      </P>
      <p class="key-value">
        Profit margin<br>
        <span>50%</span>
      </p>
    </div>
  </div>

  <div class="chart">

    <summary-title
    class="title"
    title="Profits overview"
    ></summary-title>
    <form action="">
      <div class="form-group">
        <label for="">Year</label>
        <select name="" id="">
          <option value="">2024</option>
          <option value="">2024</option>
          <option value="">2024</option>
        </select>
      </div>
      <div class="form-group">
        <label for="">Chart type</label>
        <select name="" id="">
          <option value="">line</option>
          <option value="">Bar</option>
        </select>
      </div>
    </form>
    <img :src="line" alt="graph">
  </div>

</main>

</template>
<script>
  import SummaryTitle from "./ui/summary/SummaryTitle.vue";
  import ThumbnailVue from "./ui/summary/ThumbnailVue.vue";
  import { sum } from "../util/helpers";
  import { sumSale } from "../util/helpers";
  import { TOMILLISECS } from "../util/config";
  import pie from "../assets/pie1.png";
  import line from "../assets/line-chart.png";
  export default {
    name: "GeneralSummary",
    components: { SummaryTitle, ThumbnailVue },
    data() {
      return {
        today: new Date(new Date().setHours(0, 0, 0, 0)).getTime(),
        yesterday:
          new Date(new Date().setHours(0, 0, 0, 0)).getTime() - 1 * TOMILLISECS,
        allProduce: this.$store.state.produce.produce,
        allSales: this.$store.state.sales.sales,
        pie: pie,
        line: line
      };
    },
    computed: {
      produce() {
        return Intl.NumberFormat("en-GB").format(
          sum(this.todayData(this.allProduce))
        );
      },

      sales() {
        const data =
          sumSale(this.todayData(this.allSales)) -
          sumSale(this.unsold(this.today));

        return Intl.NumberFormat("en-GB").format(data);
      },
    },
    methods: {
      todayData(data) {
        return data && data.filter((item) => item.date === this.today);
      },

      //PRODUCE
      prodDiff() {
        const prevData = sum(
          this.allProduce.filter((item) => item.date === this.yesterday)
        );

        const currData = this.produce.toString().replace(/,/g, "");

        const difference = +currData - prevData;

        if (difference > 0)
          return "+ " + Intl.NumberFormat("en-GB").format(difference);
        if (difference < 0)
          return "- " + Intl.NumberFormat("en-GB").format(Math.abs(difference));

        return difference;
      },

      //SALES

      //Category Data
      unsold(date) {
        const data = this.allSales;

        return (
          data &&
          data.filter((item) => {
            return item.category === "unsold" && item.date === date;
          })
        );
      },
      salesDiff() {
        const prevData =
          sumSale(
            this.allSales.filter((item) => item.date === this.yesterday)
          ) - sumSale(this.unsold(this.yesterday));

        const currData = this.sales.toString().replace(/,/g, "");

        const difference = +currData - prevData;

        if (difference > 0)
          return "+ " + Intl.NumberFormat("en-GB").format(difference);
        if (difference < 0)
          return "- " + Intl.NumberFormat("en-GB").format(Math.abs(difference));

        return difference;
      },
    },
  };
</script>
<style lang="scss" scoped>
main{
  display: flex;
  flex-wrap: wrap;

  .today{
    flex-basis: 100%;
    display: flex;
    flex-wrap: wrap;
    margin-bottom: 8vh;
    justify-content: flex-start;
    background-color: var(--background-grey);
    padding: 1rem;
    border-radius: 1rem;
    padding-bottom: 2rem;
    

    .title{
      flex-basis: 100%;
    }
    .thumbnails-container{
      padding: 0.5rem;
      justify-content: space-evenly;
      flex-basis: 60%;
      margin: 0;
    }
    .today-averages{
      flex-basis: 30%;
      display: flex;
      flex-wrap: wrap;
      justify-content: space-evenly;

      p{
      margin: 0.1rem;
      width: 10vw;
      text-align: center;
      padding: 1rem;
      background-color: var(--shadow-grey);

      & span {
      margin-top: 2vh;
      font-weight: 600;
      line-height: 8vh;
      font-size: 1.3rem;
      }
     }
    }
  }

  h4{
      padding: 0.4rem;
      margin-bottom: 1vh;
      text-align: center;
      flex-basis: 100%;
      border-bottom: 2px solid var(--accent1200);
    }
  .week-summary{
    flex-basis: 100%;
    display: flex;
    flex-wrap: wrap;
    background-color: var(--background-grey);
    padding: 2rem;
    border-radius: 1rem;
    justify-content: space-between;
    margin-bottom: 6vh;
    
    table{
      flex-basis: 50%;
      background-color: var(--white);
      margin: 0;
      border-top: 1px solid var(--light-grey);
      padding-top: 1rem;
      font-size: 0.88rem;
      padding: 1rem;
      
    }
    .pie{
      flex-basis: 40%;
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      
     img{
        width: 10rem;
      }
    }
  }

  .averages {
    flex-basis: 100%;
    display: flex;
    flex-wrap: wrap;
    margin-bottom: 10vh;
    justify-content: space-between;
    
    h4{
      border-bottom: none;
      background-color: var(--background-grey);
      text-align: left;
    }

    p{
      margin: 0.1rem;
      min-width: 10vw;
      text-align: center;
      padding: 1rem;
      border-right: 1.5px solid var(--light-grey);

      & span {
      margin-top: 2vh;
      font-weight: 600;
      line-height: 5vh;
      font-size: 1.3rem;
      }
    }

    .key-value{
      color: var(--accent400);
    }
    .farm-averages{
      display: flex;
      flex-wrap: wrap;
      flex-basis: 45%;

      p{
        background-color: var(--accent1300);
      }
      
    }
    .biz-averages{
      display: flex;
      flex-wrap: wrap;
      flex-basis: 45%;
    }
  }

  .chart {
    flex-basis: 100%;
    display: flex;
    flex-wrap: wrap;
    margin-bottom: 10vh;
    background-color: var(--background-grey);
    padding: 1rem;

    form{
      flex-basis: 100%;
      display: flex;
      flex-wrap: nowrap;
      padding: 0;
      margin-bottom: 3vh;
      margin-top: 3vh;
      background-color: transparent;
      border: none;
      justify-content: flex-start;

      .form-group{
        display: flex;
        flex-wrap: nowrap;
        padding: 0;
        flex-basis: 30%;
        margin: 0;
      
        label{
          width: fit-content;
          font-size: 0.88rem;
          line-height: 8vh;
          margin-right: 1.5vw;
          padding: 0;
        }
        select{
          width: fit-content;
          min-width: 10vw;
          text-align: center;
          font-size: 1rem;
          outline: none;
          border: 1px solid var(--light-grey);

        }
      }
    }

    img{
      border: 1px solid var(--background-grey);
      margin-left: 100px;
      width: 600px
    }

  }


}

.thumbnails-container{
  
  .thumbnail {
    background-color: transparent;
    border: none;
    border-radius: 0;
    box-shadow: none;
    height: 15vh;
    padding: 0;
    margin-top: 0;
    font-size: 0.88rem;
  }
  span{
    font-size: 1.5rem;
    color: var(--primary400);
    font-weight: bold;
    margin-left: 1vw;
  }
  
 
}

.week-summary{
   
  h4{
    color: var(--primary600);
    padding-bottom: 1vh;
  }

  .week-summary table {
    

    td{
      padding-right: 1rem;
      
    }

    th{
      padding-right: 1rem;
      padding-bottom: 1rem;
      color: var(--primary600);
    }

    tr{
      padding: 2rem;
      color: var(--dark-grey);

    }

    .row-category{
      font-weight: 500;
    }

   
  }
}


.last-row{
      
  & td{
    border-top: 2px solid var(--accent1200);
    padding-top: 0.3rem;
  }
}

.title{
    margin-top: -4vh;
    margin-bottom: 0;
  }
</style>
