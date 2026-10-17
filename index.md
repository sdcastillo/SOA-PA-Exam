---
layout: default
title: SOA Exam PA Notes
description: Open-source R notes for the Society of Actuaries Predictive Analytics exam.
samwiki: true
---

<section class="sw-lede" aria-labelledby="sw-about-title">
  <div class="sw-lede-copy">
    <h2 id="sw-about-title">Exam PA, worked in the open</h2>
    <p>These notes are an open-source record of preparing for the Society of Actuaries Predictive Analytics exam, written for the June 2019 sitting. The written notes are a <a href="{{ '/Exam%20PA%20Notes%20-%206-30-2019.pdf' | relative_url }}">PDF</a> and a <a href="{{ '/Exam%20PA%20Notes%20-%206-30-2019.onepkg' | relative_url }}">OneNote package</a> dated June 30, 2019, covering exam content, strategy, modeling methods, and code snippets. Exam PA was a five-hour computer-based project. A candidate received a business problem and a data set, then had to explore the data, fit and compare models in R, and write a report a non-modeler could use. The modules behind that sitting covered data types and graphics, principal components and clustering, generalized linear models, trees and random forests, and how to validate a model and explain it. The notes come from a first attempt that scored 7, about 100 hours of study, taken in the same window as a first sitting of STAM.</p>
    <p>The code uses the exam stack. dplyr and ggplot2 handle cleaning and graphics. ggplot2 is the graphics tool in Healy (2018), the visualization text on the current syllabus, and the R the modules expect is in Lander (2017). caret builds stratified train and test splits (Kuhn 2008). rpart fits trees from the CART procedure (Breiman et al. 1984). glm, and glmnet on the student-success solution, fit generalized linear models (Nelder and Wedderburn 1972; Friedman, Hastie, and Tibshirani 2010). Factor coding is part of the model. Reference levels move to the most common category so the intercept is a real baseline. Thin levels are collapsed before they become dummy variables. caret::dummyVars is run at full rank when a factor is expanded, so the design matrix does not pick up a redundant column.</p>
    <p>Hospital readmissions is a binary classification. The target is Readmission.Status. Three submissions clean length of stay, age, emergency-room use, and HCC risk score, then fit binomial GLMs. The last submission compares logit, probit, cauchit, and complementary log-log links, with a Gender-by-Race interaction as a worked example. Logit and probit are the binomial links in Nelder and Wedderburn (1972). Complementary log-log and cauchit are the other links in R’s binomial family. Discrimination is read from an ROC curve and AUC (Fawcett 2006). A later task prices errors at a probability cutoff of 0.075 and builds a confusion matrix, which is the right comparison when a missed readmission costs more than a false alarm (Fawcett 2006).</p>
    <p>The December 2018 Miners Union practice predicts injury counts, with employee hours as exposure. That sitting used a less structured project format than June 2019. Poisson trees go through rpart’s poisson method, with cbind(EMP_HRS_TOTAL/2000, NUM_INJURIES) on the left-hand side, then pruning on cross-validated error. The Poisson GLM uses a log link and an offset of log(hours/2000), so the coefficients are log injury rates and a response-scale prediction is an expected count. An offset of log exposure is the usual way to turn a Poisson GLM into a rate model (Goldburd et al. 2025). Fit is scored with RMSE, MAE, and a Poisson loglikelihood that replaces nonpositive tree predictions before the log is taken.</p>
    <p>Student academic success is the first form of the June 2019 sample project, before the report structure was tightened. About 585 students are flagged pass or fail from final grade G3, with pass at 10 or above. Period grades G1 and G2, and absences, are dropped so they cannot leak the outcome. The comparison is a classification tree (Breiman et al. 1984), a caret random forest (Breiman 2001; Kuhn 2008), and a binomial logit GLM (James et al. 2021). The solution file also fits an L1-penalized logistic regression in glmnet (alpha = 1), with the penalty chosen by cross-validation (Friedman, Hastie, and Tibshirani 2010). Tay, Narasimhan, and Hastie (2023) later extended that coordinate-descent path to the rest of the GLM family. The syllabus has moved since 2019 (Society of Actuaries 2026). Check the <a href="https://www.soa.org/education/exam-req/edu-exam-pa-detail/">SOA Exam PA page</a> before using these notes for a current sitting.</p>
  </div>
  <aside class="sw-find" aria-labelledby="sw-find-title">
    <h2 id="sw-find-title">In this repository</h2>
    <ul>
      <li><strong><a href="{{ '/Exam%20PA%20Notes%20-%206-30-2019.pdf' | relative_url }}">Written notes</a></strong> PDF from June 30, 2019, on exam content, strategy, modeling methods, and code snippets. The same notes are in a <a href="{{ '/Exam%20PA%20Notes%20-%206-30-2019.onepkg' | relative_url }}">OneNote package</a>.</li>
      <li><strong><a href="https://github.com/sdcastillo/SOA-PA-Exam/tree/master/Hospital%20Readmissions">Hospital readmissions</a></strong> Three submissions on a binary readmission flag, link functions, ROC, and a cost cutoff.</li>
      <li><strong><a href="https://github.com/sdcastillo/SOA-PA-Exam/tree/master/Miners%20Union">Miners Union</a></strong> December 2018 practice. Poisson tree and Poisson GLM with an exposure offset.</li>
      <li><strong><a href="https://github.com/sdcastillo/SOA-PA-Exam/tree/master/Student%20Academic%20Success">Student academic success</a></strong> June 2019 sample. Tree, random forest, logit GLM, and penalized logistic regression.</li>
      <li><strong>Factor helpers</strong> Relevel to the modal category, collapse rare levels, and dummy-code at full rank.</li>
      <li><strong>What the exam asked for</strong> A written recommendation, not only a model object. Validation and the business cutoff belong in the report.</li>
      <li><strong><a href="#sw-articles">Recent articles</a></strong> The April 2026 syllabus, two SOA research reports from 2024 and 2025, and the 2023 glmnet paper, with full citations below.</li>
    </ul>
  </aside>
</section>

<section class="sw-articles" id="sw-articles" aria-labelledby="sw-articles-title">
  <h2 id="sw-articles-title">Recent articles</h2>
  <p>These notes record the June 2019 sitting. The current exam is the April 2026 syllabus: a three-hour-and-thirty-minute written exam on a computer with Word and Excel. Starting with the April 2023 administration, R and RStudio are not available, and the exam supplies the code and output each task needs (Society of Actuaries 2026). Required reading is still James, Witten, Hastie, and Tibshirani, either the 2013 first edition or the 2021 second edition, plus Frees (2010), Lander (2017), and Healy (2018). The syllabus table is the list of assigned sections for a current sitting.</p>
  <ul>
    <li>Adams, Edwalds, Holland, Niemerg, and Yu (2024) give a reusable workflow for life-insurance experience studies. On 2013–2017 mortality data they fit a GLM, an elastic-net GLM, and gradient-boosted trees, then compare them with lift, an ordered Lorenz curve and Gini, feature importance, and SHAP. The repository behind the report is <a href="https://github.com/Society-of-actuaries-research-institute/RILEC">RILEC</a>.</li>
    <li>Patil, Descy, and Matson (2025) review predictive analytics in life and annuity reinsurance. The techniques they describe include random forests, gradient boosting, and support vector machines, alongside the data, regulatory, and explainability constraints those models run into.</li>
    <li>Tay, Narasimhan, and Hastie (2023) extend the glmnet coordinate-descent path from the linear, logistic, and multinomial cases in Friedman, Hastie, and Tibshirani (2010) to the rest of the GLM family. The student-success file’s L1 logistic (alpha = 1) is the 2010 path. The 2023 paper is the citation for a penalized GLM on a count or other non-binomial response.</li>
  </ul>
  <h3 id="sw-refs-title">References</h3>
  <ol class="sw-refs">
    <li>Adams, Philip L., Cynthia Edwalds, Brian Holland, Michael Niemerg, and Haofeng Yu. 2024. “A Predictive Analytics Framework: A Reusable Workflow for Experience Analysis.” Society of Actuaries Research Institute, November. <a href="https://www.soa.org/resources/research-reports/2024/predictive-analytics-framework/">https://www.soa.org/resources/research-reports/2024/predictive-analytics-framework/</a></li>
    <li>Breiman, Leo. 2001. “Random Forests.” <em>Machine Learning</em> 45 (1): 5–32. <a href="https://doi.org/10.1023/A:1010933404324">https://doi.org/10.1023/A:1010933404324</a></li>
    <li>Breiman, Leo, Jerome Friedman, Richard Olshen, and Charles Stone. 1984. <em>Classification and Regression Trees</em>. Belmont, CA: Wadsworth International Group.</li>
    <li>Fawcett, Tom. 2006. “An Introduction to ROC Analysis.” <em>Pattern Recognition Letters</em> 27 (8): 861–874. <a href="https://doi.org/10.1016/j.patrec.2005.10.010">https://doi.org/10.1016/j.patrec.2005.10.010</a></li>
    <li>Frees, Edward W. 2010. <em>Regression Modeling with Actuarial and Financial Applications</em>. New York: Cambridge University Press. ISBN 978-0-521-13596-2.</li>
    <li>Friedman, Jerome, Trevor Hastie, and Robert Tibshirani. 2010. “Regularization Paths for Generalized Linear Models via Coordinate Descent.” <em>Journal of Statistical Software</em> 33 (1): 1–22. <a href="https://doi.org/10.18637/jss.v033.i01">https://doi.org/10.18637/jss.v033.i01</a></li>
    <li>Goldburd, Mark, Anand Khare, Dan Tevet, and Dmitriy Guller. 2025. <em>Generalized Linear Models for Insurance Rating</em>. 2nd ed. (2025 revision). CAS Monograph Series Number 5. Arlington, VA: Casualty Actuarial Society. <a href="https://www.casact.org/monograph/cas-monograph-no-5">https://www.casact.org/monograph/cas-monograph-no-5</a></li>
    <li>Healy, Kieran. 2018. <em>Data Visualization: A Practical Introduction</em>. Princeton, NJ: Princeton University Press. ISBN 978-0-691-18162-2. <a href="https://socviz.co/">https://socviz.co/</a></li>
    <li>James, Gareth, Daniela Witten, Trevor Hastie, and Robert Tibshirani. 2021. <em>An Introduction to Statistical Learning: With Applications in R</em>. 2nd ed. New York: Springer. The 2013 first edition remains acceptable on the exam. <a href="https://www.statlearning.com/">https://www.statlearning.com/</a></li>
    <li>Kuhn, Max. 2008. “Building Predictive Models in R Using the caret Package.” <em>Journal of Statistical Software</em> 28 (5): 1–26. <a href="https://doi.org/10.18637/jss.v028.i05">https://doi.org/10.18637/jss.v028.i05</a></li>
    <li>Lander, Jared P. 2017. <em>R for Everyone: Advanced Analytics and Graphics</em>. 2nd ed. Boston: Addison-Wesley. ISBN 978-0-13-454692-6.</li>
    <li>Nelder, J. A., and R. W. M. Wedderburn. 1972. “Generalized Linear Models.” <em>Journal of the Royal Statistical Society, Series A</em> 135 (3): 370–384. <a href="https://doi.org/10.2307/2344614">https://doi.org/10.2307/2344614</a></li>
    <li>Patil, Sandeep, Michael Descy, and Tricia Matson. 2025. “Predictive Analytics in Life and Annuity Reinsurance.” Society of Actuaries Research Institute, June. <a href="https://www.soa.org/globalassets/assets/files/resources/research-report/2025/predictive-life-reinsurance/2025-06-ml242.pdf">https://www.soa.org/globalassets/assets/files/resources/research-report/2025/predictive-life-reinsurance/2025-06-ml242.pdf</a></li>
    <li>Society of Actuaries. 2026. “Predictive Analytics Exam—April 2026.” Syllabus. <a href="https://www.soa.org/globalassets/assets/files/edu/2026/spring/syllabi/2026-04-exam-pa-syllabus.pdf">https://www.soa.org/globalassets/assets/files/edu/2026/spring/syllabi/2026-04-exam-pa-syllabus.pdf</a></li>
    <li>Tay, J. Kenneth, Balasubramanian Narasimhan, and Trevor Hastie. 2023. “Elastic Net Regularization Paths for All Generalized Linear Models.” <em>Journal of Statistical Software</em> 106 (1): 1–31. <a href="https://doi.org/10.18637/jss.v106.i01">https://doi.org/10.18637/jss.v106.i01</a></li>
  </ol>
</section>

<section class="sw-videos" aria-labelledby="sw-videos-title">
  <h2 id="sw-videos-title">Videos</h2>
  <p>Sam Castillo's Exam PA videos: study tips, a live webinar, and worked practice-exam solutions, including the hospital readmissions project in this repository.</p>
  <div style="position:relative;padding-top:56.25%;margin:1rem 0;">
    <iframe src="https://rumble.com/embed/v7dies4/" title="Practice Exam - Hospital Readmissions - Solution" style="position:absolute;inset:0;width:100%;height:100%;border:0;" allowfullscreen loading="lazy"></iframe>
  </div>
  <ul>
    <li><a href="https://rumble.com/v7forpm-practice-exam-hospital-readmissions-solution.html">Practice Exam - Hospital Readmissions - Solution</a> (Rumble)</li>
    <li><a href="https://rumble.com/v7forfo-predictive-analytics-exampa-practice-exam-2-video-solution-reupload.html">Predictive Analytics (ExamPA) Practice Exam 2 Video Solution (Reupload)</a> (Rumble)</li>
    <li><a href="https://rumble.com/v7foqki-bicycle-sharing-demand-prediction-tutorial-exam-solution-walkthrough.html">Bicycle Sharing Demand Prediction Tutorial - Exam Solution Walkthrough</a> (Rumble)</li>
    <li><a href="https://rumble.com/v7fosfw-live-webinar-exam-pa-machine-learning-secrets-you-cant-miss.html">Live Webinar: Exam PA Machine Learning Secrets You Can't Miss!</a> (Rumble)</li>
    <li><a href="https://www.youtube.com/watch?v=F2okL4a2YcM">Unleash Your Potential: Step-by-Step Study Tips for Exam PA (2021 Update)!</a> (YouTube)</li>
    <li><a href="https://www.youtube.com/watch?v=FE5-pKaBHrg">Unlock the Power of Cost Complexity Pruning with Ease!</a> (YouTube)</li>
    <li><a href="https://www.youtube.com/watch?v=xDJXuoM6ZCI">Mastering GLM Link Functions: A Comprehensive Guide</a> (YouTube)</li>
  </ul>
</section>
