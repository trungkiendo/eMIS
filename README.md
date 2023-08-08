/* Định nghĩa mốc thời gian start_time và end_time */
%let start_time = '08:30:00'dt;
%let end_time = '2023-08-08 16:45:00'dt;

/* Định nghĩa danh sách các ngày nghỉ lễ */
%let holiday_list = ('2023-01-01'dt, '2023-04-10'dt, '2023-09-04'dt);

/* Tính số phút giữa hai mốc thời gian */
%let minutes = intck('MINUTE', &start_time, &end_time);

/* Loại bỏ các ngày cuối tuần */
%let start_weekday = weekday(&start_time);
%let end_weekday = weekday(&end_time);

%if &start_weekday in (1, 7) or &end_weekday in (1, 7) %then %do;
    %let weekend_minutes = intck('MINUTE', intnx('DAY', &start_time, 1, 'END'), &end_time) + 1;
    %let minutes = &minutes - &weekend_minutes;
%end;

/* Loại bỏ các ngày nghỉ lễ */
%let holiday_minutes = 0;

%do %while (%length(&holiday_list) > 0);

    %let holiday_date = %scan(&holiday_list, 1);

    %if &start_time <= &holiday_date <= &end_time %then %do;
        %let holiday_minutes = %eval(&holiday_minutes + 1440);
    %end;

    %let holiday_list = %substr(&holiday_list, %eval(%length(&holiday_date) + 2));

%end;

/* Trừ đi số phút của các ngày nghỉ lễ */
%let minutes = %eval(&minutes - &holiday_minutes);

/* In kết quả */
%put Số phút giữa start_time và end_time (loại bỏ ngày nghỉ lễ) là &minutes;
