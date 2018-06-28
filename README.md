# 15-Days-of-Learning-SQL
My answer of the "15 Days of Learning SQL" on HackerRank


select tabP.submission_date, tabP.hack_id_cnt, tabQ.first_id, hackers.name </br>
from </br>
  (select tabC.submission_date, count(tabC.hacker_id) as hack_id_cnt </br>
  from </br>
    (select tabA.submission_date, tabA.hacker_id, tabA.dailyFreq </br>
    from </br>
      (select submission_date, hacker_id, count(hacker_id) as dailyFreq from submissions </br>
      group by submission_date, hacker_id) as tabA </br>
    left join </br>
      (select submission_date, hacker_id, count(hacker_id) as dailyFreq from submissions </br>
      group by submission_date, hacker_id) as tabB </br>
    on tabA.hacker_id = tabB.hacker_id and tabA.submission_date >= tabB.submission_date </br>
    group by tabA.submission_date, tabA.hacker_id, tabA.dailyFreq </br>
    having day(tabA.submission_date) = count(tabB.submission_date)) as tabC </br>
  group by tabC.submission_date) as tabP </br>
left join </br>
    (select tabG.submission_date, min(tabG.hacker_id) as first_id </br>
    from </br>
      (select submission_date, hacker_id, count(hacker_id) as hack_cnt </br>
      from submissions </br>
      group by submission_date, hacker_id) as tabG </br>
    inner join </br>
      (select submission_date, max(hack_cnt) as max_cnt </br>
      from </br>
      ( </br>
        select submission_date, hacker_id, count(hacker_id) as hack_cnt </br>
        from submissions </br>
        group by submission_date, hacker_id </br>
      ) as tabF </br>
    group by submission_date) as tabH </br>
    on tabG.submission_date = tabH.submission_date and tabG.hack_cnt = tabH.max_cnt </br>
  group by tabG.submission_date) as tabQ </br>
using(submission_date) </br>
left join </br>
hackers </br>
on tabQ.first_id = hackers.hacker_id </br>
order by tabP.submission_date </br>

