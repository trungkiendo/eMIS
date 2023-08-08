/* Định nghĩa mốc thời gian start_time và end_time */
%let start_time = '08:30:00'dt;
%let end_time = '2023-08-08 16:45:00'dt;

/* Định nghĩa danh sách các ngày nghỉ lễ */
%let holiday_list = ('2023-01-01'dt, '2023-04-10'dt, '2023-09-04'dt);

/* Tính số phút giữa hai mốc thời gian */
%let minutes = intck('MINUTE', &start_time, &end_time);

/* Kiểm tra nếu có ngày cuối tuần */
%let start_weekday = weekday(&start_time);
%let end_weekday = weekday(&end_time);

%if &start_weekday in (1, 7) or &end_weekday in (1, 7) %then %do;
    /* Trừ đi số phút từ start_time đến cuối ngày */
    %let start_time = intnx('DAY', &start_time, 1, 'END');
    %let weekend_minutes = intck('MINUTE', &start_time, &end_time) + 1;

    /* Trừ đi số phút từ đầu ngày đến end_time */
    %let end_time = intnx('DAY', &end_time, -1, 'BEGIN');
    %let weekend_minutes = &weekend_minutes + intck('MINUTE', &start_time, &end_time) + 1;

    %let minutes = &minutes - &weekend_minutes;
%end;

/* Kiểm tra nếu start_time hoặc end_time nằm ngoài khoảng thời gian từ 8:00 đến 17:00 */
%let start_hour = hour(&start_time);
%let end_hour = hour(&end_time);

%if &start_hour < 8 %then %do;
    /* Đặt start_time là 8:00 */
    %let start_time = dhms(datepart(&start_time), 8, 0, 0);
%end;

%if &end_hour > 17 %then %do;
    /* Đặt end_time là 17:00 */
    %let end_time = dhms(datepart(&end_time), 17, 0, 0);
%end;

%if &start_time < &end_time %then %do;
    /* Trừ đi số phút từ 8:00 đến start_time */
    %let start_minutes = intck('MINUTE', dhms(datepart(&start_time), 8, 0, 0), &start_time);

    /* Trừ đi số phút từ end_time đến 17:00 */
    %let end_minutes = intck('MINUTE', &end_time, dhms(datepart(&end_time), 17, 0, 0));

    %let minutes = &minutes - &start_minutes - &end_minutes;
%end;

/* Kiểm tra và trừ đi số phút của các ngày nghỉ lễ */
%let holiday_minutes = 0;
%let i = 1;

%do %while (%scan(&holiday_list, &i, ',' ne ));

    %let holiday_date = %scan(&holiday_list, &i, ',');

    %if &start_time <= &holiday_date <= &end_time %then %do;
        /* Trừ đi số phút của ngày nghỉ lễ */
        %let holiday_minutes = %eval(&holiday_minutes + 1440);
    %end;

    %let i = %eval(&i + 1);
%end;

/* Trừ đi số phút của các ngày nghỉ lễ */
%let minutes = %eval(&minutes - &holiday_minutes);

/* In kết quả */
%put Số phút giữa start_time và end_time (loại bỏ ngày nghỉ lễ) là &minutes;
