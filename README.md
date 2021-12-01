# First_Project

SET serveroutput ON;


DECLARE

    -----declare weak cursor definition to hold work details
type weak_refcursor IS ref CURSOR;
    cur_var weak_refcursor;
    
    ---define variables
    v_cl test_table.class%type;
    v_pre test_details.pretestscore%type;
    v_pos test_details.posttestscore%type;
    v_ch test_details.change%type;

    v_m test_table.monthlyreport%type;

    v_q test_details.question%type;
    v_p test_details.pretestscore%type;
    v_po test_details.posttestscore%type;
    v_c test_details.change%type;

    /*v_a test_details.pretestscore%type;
    v_b test_details.posttestscore%type;
    v_ca  test_details.change%type; */

    ----define an explicit cursor named cur_jules
    CURSOR cur_jules IS
      SELECT class, CURSOR
        (SELECT question,
          pretestscore,
          posttestscore,
          change
        FROM test_details
        WHERE class = class2
        ) FROM test_table t
    WHERE monthlyreport ='&monthlyreport';

  BEGIN
    ----Select statement AVG---------
    SELECT AVG(pretestscore), AVG(posttestscore), AVG(change)
    INTO v_p, v_po, v_c FROM test_details
    WHERE monthlyreport2 ='&monthlyreport2';

    ----------Open Cursor----------------
    OPEN cur_jules;
    ---begin the loop for the 1st cursor
    LOOP
    ---retrieve the next classname---
    FETCH cur_jules INTO v_cl, cur_var;

    ---exit the loop if no data remains---
    EXIT  WHEN cur_jules%notfound;

    ---output class names---
    dbms_output.put_line(v_cl);
    dbms_output.put_line(' ');

    -----begin the loop for the 2nd cursor
    LOOP
    ---retrieve the next work (set of questions, Pretestscores, Posttestscores)---
    FETCH cur_var INTO v_q, v_pre, v_pos, v_ch;

    ---exit the loop if no data found---
    EXIT  WHEN cur_var%notfound;

    ---output Question, Pretestscore, Posttestscore---
    dbms_output.put_line('                           ' ||v_q||'   /   '||v_pre||'   /   '||v_pos||'   /   '||v_ch);
    dbms_output.put_line('  ');
    END LOOP;

    ---dbms_output.put_line ('            Average For This class:----------->   '||v_a||'  '||v_b||'  '||v_ca);
    dbms_output.put_line(chr(10));
    ---dbms_output.put_line(chr(10));
    END LOOP;

    ---Display Average-----------
    dbms_output.put_line ('                           &monthlyreport2 Average :----------->   '||v_p||'     '||v_po||'     '||v_c);
    ------close cursor--
    CLOSE cur_jules;
  END;
