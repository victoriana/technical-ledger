# NYL POC Summary# NYL POC Summary

I built a small AI program in Python that checks and improves its own answers. It runs on Amazon's cloud (AWS SageMaker). A tool called LangGraph controls the steps, Claude does the thinking and writing, and the data comes from Databricks, which holds approved, controlled data.

It works as a simple agentic workflow: instead of answering once, it loops. It looks at a data table, writes a rough first answer, reviews it, and decides whether it's good enough. If not, it rewrites it, and it stops after a set number of tries. At the end it gives you the final answer plus a record of what changed at each step and why.
