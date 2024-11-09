
<!-- src/routes/+page.svelte -->
<script>
  import UserBehaviorChart from './UserBehaviorChart.svelte';
</script>
  
<div>
  <UserBehaviorChart />
</div>

<div class="description">
  <p>
      We sourced and extracted a dataset from Kaggle titled 
      <strong>"Mobile Device Usage and User Behavior"</strong>, 
      which contains 700 data samples with metrics such as app usage time, 
      screen-on time, battery drain, and data consumption. Each entry is 
      categorized into one of five user behavior classes, ranging from light 
      to extreme usage. The dataset consists of 700 rows and 11 columns, and 
      we focused on the relevant columns, <strong>Age</strong> and 
      <strong>App Usage Time</strong>, for our visualization.
  </p>

  <p>
      The key questions we aimed to answer through this visualization were: 
      <em>How does daily app usage time vary across different age groups?</em> 
      Are there any notable trends or patterns in app usage intensity between 
      younger and older users? Our goal was to visualize the distribution of 
      app usage time on mobile devices across age groups, helping to understand 
      the correlation between age and mobile device usage.
  </p>

  <p>
      We chose to use a scatter plot to represent the data, where each point 
      corresponds to a specific user. For example, users aged 40 have app usage 
      times ranging from 50 minutes per day to 550 minutes per day. The x-axis 
      represents age, and the y-axis shows app usage time in minutes per day. 
      We also included a title with the primary question we aimed to answer 
      through the graph.
  </p>

  <p>
      A key interactive feature of our visualization allows users to filter the 
      data by age group or app usage time range using a filtering bar. This enables 
      users to focus on a specific subset of the data and remove irrelevant points. 
      Initially, we considered using a sliding bar to let users select an age or 
      app usage time range. However, we found this approach too limiting, as it 
      only allowed users to set age ranges from 0 to any specific age. To address 
      this, we switched to an input box, allowing users to specify custom age ranges, 
      such as "18–25," in the filter.
  </p>

  <p>
      Our steps to develop the interactive visualization included sourcing the dataset, 
      defining the key questions, setting up the Svelte and D3.js environments, implementing 
      interactivity with D3.js, and deploying the project on GitHub Pages. The project took 
      approximately four days to complete. Our group, consisting of <strong>Yingshan Xie</strong> 
      and <strong>Rongrong Xu</strong> , split the work evenly and collaborated for about 2–3 hours per day. Rongrong was 
      responsible for finding the dataset, defining the questions, and writing the summaries, 
      while Yingshan handled setting up the D3.js environment and implementing the interactive 
      features.
  </p>
</div>

<style>
  .description {
      max-width: 800px;
      margin: 0 auto;
      line-height: 1.6;
      font-size: 1rem;
      color: #333;
  }


  p {
      margin-bottom: 1em;
  }
</style>
  