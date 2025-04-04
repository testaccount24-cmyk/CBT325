# CBT325
Converted to GitHub via [cbt2git](https://github.com/wizardofzos/cbt2git)

This is still a work in progress. GitHub repos will be deleted and created during this period...

```
//***FILE 325 IF FROM WELLS FARGO BANK AND CONTAINS THE FOLLOWING   *   FILE 325
//*           PROGRAMS AND TSO COMMANDS.  ALL CODE IS CURRENT AS OF *   FILE 325
//*           SEP 1986,  MVS / XA 2.1.5.                            *   FILE 325
//*           THIS FILE IS IN IEBUPDTE SYSIN FORMAT.                *   FILE 325
//*           SEE THE MEMBER CALLED $$DOC FOR ADDITIONAL            *   FILE 325
//*           INFORMATION.                                          *   FILE 325
//*                                                                 *   FILE 325
//*           (Note:  PRINTOFF was fixed for z/OS.)                 *   FILE 325
//*                                                                 *   FILE 325
//*         DARTH    - THE 'DUMP ACTIVITY REPORTER / TAPE HANDLER'  *   FILE 325
//*                    UTILITY THAT PROCESSES SYS1.DUMP DATASETS.   *   FILE 325
//*                    DARTH RUNS AS A STARTED TASK THAT WAKES UP   *   FILE 325
//*                    EVERY 15 MINUTES TO CHECK ALL DEFINED DUMP   *   FILE 325
//*                    DATASETS.  WHEN A DUMP IS PRESENT, A TAPE    *   FILE 325
//*                    IS DYNAMICALLY ALLOCATED, THE DUMP IS        *   FILE 325
//*                    OFFLOADED, AND THE DATASET IS RESET.  THEN   *   FILE 325
//*                    A CONTROL DATASET IS UPDATED TO RECORD THE   *   FILE 325
//*                    SYSTEM ID, DATE, AND TIME WHERE THE DUMP     *   FILE 325
//*                    OCCURRED, THE TAPE VOLUME IDETIFICATION,     *   FILE 325
//*                    AND THE ACTUAL TITLE FROM THE DUMP.          *   FILE 325
//*                                                                 *   FILE 325
//*         IEFUTL   - WELLS FARGO'S VERSION OF THE SMF USER TIME   *   FILE 325
//*                    LIMIT EXIT.  YOU WILL NOT BE ABLE TO USE     *   FILE 325
//*                    THIS CODE AS IT STANDS BECAUSE OF SOME       *   FILE 325
//*                    INSTALLATION DEPENDENCIES THAT ARE NOT       *   FILE 325
//*                    SHIPPED.                                     *   FILE 325
//*                                                                 *   FILE 325
//*                      THIS IS AN EXAMPLE OF HOW TO DISCONNECT A  *   FILE 325
//*                    TSO USER RATHER THAN CANCEL WHEN WAIT TIME   *   FILE 325
//*                    IS EXCEEDED.  THE USER THEN HAS HOWEVER      *   FILE 325
//*                    MUCH TIME AS YOU ALLOW IN YOUR RECONLIM=     *   FILE 325
//*                    PARAMETER TO LOGON RECONNECT BEFORE VTAM     *   FILE 325
//*                    AUTOMATICALLY CANCELS THE ADDRESS SPACE.     *   FILE 325
//*                    THE TERMINAL BECOMES IMMEDIATELY AVAILABLE   *   FILE 325
//*                    FOR USE BY OTHER IDS, AND THE DISCONNECTED   *   FILE 325
//*                    ID REMAINS SWAPPED OUT.                      *   FILE 325
//*                                                                 *   FILE 325
//*                      SEE THE CODE THAT REFERS TO VTAM COMMAND:  *   FILE 325
//*                      V NET,TERM, ...                            *   FILE 325
//*                                                                 *   FILE 325
//*                    NOTE ===> THIS PERFORMS THE SAME FUNCTION    *   FILE 325
//*                      THROUGH OPERATOR COMMAND THAT ANY USER     *   FILE 325
//*                      CAN INVOKE THROUGH KEYBOARD ACTION.  IF    *   FILE 325
//*                      YOU ARE NOT FAMILIAR WITH CONDITIONAL      *   FILE 325
//*                      LOGOFF, READ "INVOKING UNFORMATTED SYSTEM  *   FILE 325
//*                      SERVICE TYPE(COND) TO FORCE A RECONNECT    *   FILE 325
//*                      ENVIRONMENT" IN THE TSO TERMINAL USER'S    *   FILE 325
//*                      GUIDE.                                     *   FILE 325
//*                                                                 *   FILE 325
//*         INMXZ01  - TSO/E TRANSMIT INITIALIZATION EXIT TO        *   FILE 325
//*                    PREVENT TRANSMISSION TO NON-EXISTENT TSO     *   FILE 325
//*                    USERIDS ON SAME NODE AS SENDER (JES2 EXIT 13 *   FILE 325
//*                    HANDLES NJE TRANSMISSIONS FROM OTHER NODES)  *   FILE 325
//*                    AND THEREBY KEEP UNRECEIVABLE DATA OFF JES2  *   FILE 325
//*                    SPOOL.                                       *   FILE 325
//*                                                                 *   FILE 325
//*                      THE EXIT SEARCHES THE ADDRESS              *   FILE 325
//*                    LIST FOR A LOCAL NODE, THEN CALLS            *   FILE 325
//*                    ACF2 TO VERIFY EXISTENCE OF USER.            *   FILE 325
//*                    IF NOT VALID, THE USERID IS                  *   FILE 325
//*                    REMOVED FROM THE ADDRESS LIST AND            *   FILE 325
//*                    AN ERROR MESSAGE IS ISSUED TO THE            *   FILE 325
//*                    SENDER.                                      *   FILE 325
//*                    NOTE:  REQUIRES PTF UZ39974 (OR EQUIVALENT)  *   FILE 325
//*                           BE INSTALLED FOR CORRECT FUNTIONING   *   FILE 325
//*                           OF "UNCHAINING" INVALID USERID.       *   FILE 325
//*                                                                 *   FILE 325
//*         INMXZ02  - TSO/E TRANSMIT TERMINATION EXIT TO ISSUE     *   FILE 325
//*                    EQUIVALENT OF "$HASP549 MAIL FROM" MESSAGE   *   FILE 325
//*                    TO NOTIFY RECEIVING USERS OF LOCAL NODE      *   FILE 325
//*                    TRANSMISSION.  (LOCAL TRANSMISSIONS DO NOT   *   FILE 325
//*                    PASS THROUGH JES2 EXIT 13).                  *   FILE 325
//*                                                                 *   FILE 325
//*                      THE EXIT SEARCHES THE ADDRESS LIST FOR A   *   FILE 325
//*                    LOCAL NODE, THEN VERIFIES THE OUTPUT TARGET  *   FILE 325
//*                    IS JES SYSOUT AND TRANSMISSION HAS           *   FILE 325
//*                    SUCCESSFULLY COMPLETED.  "SEND               *   FILE 325
//*                    '$HASP549...',USER=(),LOGON" IS ISSUED VIA   *   FILE 325
//*                    SVC34.                                       *   FILE 325
//*                                                                 *   FILE 325
//*         JESLOGON - A PROGRAM TO ALLOW A TSO USER TO LOGON TO    *   FILE 325
//*                    ANY SECONDARY SUBSYSTEM.  IT ACTS AS A       *   FILE 325
//*                    ONE-TIME FRONT END FOR THE STANDARD TMP,     *   FILE 325
//*                    AND IS INTENDED TO BE EXECUTED BY THE        *   FILE 325
//*                    LOGON PROCEDURE.  JOB SUBMISSIONS AND PSO    *   FILE 325
//*                    (PROCESS SYSOUT) REQUESTS ARE ALSO HANDLED   *   FILE 325
//*                    BY THE SECONDARY JES.                        *   FILE 325
//*                                                                 *   FILE 325
//*         JESMAXCC - A PAIR OF JES2 (SP2.1.5) EXITS THAT ADD TEXT *   FILE 325
//*                    TO THE $HASP165 MESSAGE GENERATED BY NOTIFY= *   FILE 325
//*                    ON THE JOB CARD OR BY THE JES2 /*NOTIFY      *   FILE 325
//*                    CONTROL CARD.  IF THE JOB DOES NOT ABEND,    *   FILE 325
//*                    THE MAXIMUM CONDITION CODE OF ALL EXECUTED   *   FILE 325
//*                    STEPS IS ADDED.  IF THE JOB ABENDS, THE      *   FILE 325
//*                    SYSTEM OR USER ABEND CODE IS ADDED:          *   FILE 325
//*                      $HASP165 YOURJOB ENDED AT NODE - MAX COND  *   FILE 325
//*                      CODE 0000                                  *   FILE 325
//*                      $HASP165 YOURJOB ENDED AT NODE - ABENDED   *   FILE 325
//*                      USER XXX                                   *   FILE 325
//*                      $HASP165 YOURJOB ENDED AT NODE - CANCELLED *   FILE 325
//*                      SYSTEM 222                                 *   FILE 325
//*                                                                 *   FILE 325
//*         NOTE     - TWO CLISTS, ONE ISPF PANEL, AND A HELP ENTRY *   FILE 325
//*                    THAT PROVIDE A FACILITY TO DELIVER PROFS     *   FILE 325
//*                    NOTES FROM A TSO SESSION:                    *   FILE 325
//*                      NOTE     - THE DRIVING CLIST, INVOKES      *   FILE 325
//*                                 WFBNOTE AND NOTEIMAC            *   FILE 325
//*                      NOTEIMAC - PDF EDIT INITIAL MACRO FOR      *   FILE 325
//*                                 SPECIAL FORMATTING              *   FILE 325
//*                      WFBNOTE  - ISPF PANEL TO COLLECT DATA FOR  *   FILE 325
//*                                 NOTE CLIST                      *   FILE 325
//*                      NOTEHELP - HELP ENTRY FOR NOTE CLIST       *   FILE 325
//*                                 (RENAME TO ==> NOTE)            *   FILE 325
//*                    THE NOTE COMMAND PROCEDURE (CLIST) USES THE  *   FILE 325
//*                    ISPF/PDF EDITOR TO BUILD AND FORMAT MAIL     *   FILE 325
//*                    "NOTES", THEN SENDS THEM TO PROFS OR TSO     *   FILE 325
//*                    USERS.  NOTE USES ISPF DIALOG SERVICES TO    *   FILE 325
//*                    COLLECT INFORMATION BY DISPLAYING PANELS,    *   FILE 325
//*                    THEREFORE NOTE MUST BE EXECUTED WHILE ISPF   *   FILE 325
//*                    IS ACTIVE.                                   *   FILE 325
//*                    THIS FACILITY WAS ORIGINALLY WRITTEN TO USE  *   FILE 325
//*                    THE TSO TRANSMIT COMMAND FOR DATA            *   FILE 325
//*                    TRANSMISSION, BUT BECAUSE PROFS CAN NOT      *   FILE 325
//*                    DECIPHER TRANSMIT CONTROL TAGS, NOTE NOW     *   FILE 325
//*                    USES A SPECIALLY MODIFIED VERSION OF THE     *   FILE 325
//*                    PRINTOFF COMMAND.  THIS PRINTOFF, WHICH      *   FILE 325
//*                    ALLOWS DEST(NODE.USER), IS PROVIDED IN THIS  *   FILE 325
//*                    PACKAGE.                                     *   FILE 325
//*                                                                 *   FILE 325
//*                    (Fixed for z/OS in two ways.  See member     *   FILE 325
//*                     called $PRINTOF.)                           *   FILE 325
//*                                                                 *   FILE 325
//*         OPCON    - OPERATOR CONSOLE MONITOR MODIFIED FOR WFB    *   FILE 325
//*                                                                 *   FILE 325
//*                    S   P   Y   (NAME CHANGED TO "OPCON" IN      *   FILE 325
//*                                THE CODE, BUT COMMENTS STILL     *   FILE 325
//*                                REFER TO "SPY")                  *   FILE 325
//*                                                                 *   FILE 325
//*                    THIS PROGRAM DISPLAYS THE CONTENTS OF ALL    *   FILE 325
//*                    ACTIVE GRAPHIC OPERATOR'S CONSOLES ON A TSO  *   FILE 325
//*                    CRT.  THE OPERATOR'S SCREEN CAN BE EITHER A  *   FILE 325
//*                    327X OR A 370-168 INTEGRATED CONSOLE.  THE   *   FILE 325
//*                    TSO USER CAN USE ANY 327X TERMINAL.          *   FILE 325
//*                                                                 *   FILE 325
//*             V3.3.2 - CORRECT SUPPORT FOR 327X MODEL 3 CONSOLE.  *   FILE 325
//*             V3.3.1 - CHANGE CONSOLE ASID TO 7 DUE TO CATALOG    *   FILE 325
//*                      ASID W/ DFP V2.                            *   FILE 325
//*               V3.3 - ADD SUPPORT FOR VIEWING CONSOLES THAT HAVE *   FILE 325
//*                      3270 EXTENDED FIELD ATTRIBUTES (E.G. 3179, *   FILE 325
//*                      3180, 3279-3B, ETC.).                      *   FILE 325
//*                    - MAKE OPCON NON-SWAPPABLE.                  *   FILE 325
//*                    - CHANGE CONSOLE ASID TO 6 FOR XA (S/370     *   FILE 325
//*                      CONASID IS 5).                             *   FILE 325
//*               V3.2 - ELIMINATE SPECIAL CHARACTER REQUIRED TO    *   FILE 325
//*                      PRECEDE OS CMD                             *   FILE 325
//*                    - ELIMINATE SECRET AUTH SVC, RESTORE MODESET *   FILE 325
//*                      AND SVC34                                  *   FILE 325
//*                    - CORRECT BUFFER ADDRESS PROBLEM WITH LINE 1 *   FILE 325
//*                      OF DISPLAY                                 *   FILE 325
//*                    - CORRECT LOOP COUNT FOR UCM BUILD ROUTINE   *   FILE 325
//*                    - ADD SUBCOMMAND A.. (AUTO W.. AFTER COMMAND *   FILE 325
//*                      ENTRY)                                     *   FILE 325
//*                    - ADD CHECK AT INITIALIZATION FOR TSO OPER   *   FILE 325
//*                      AUTHORITY                                  *   FILE 325
//*                    - MAKE COMMAND ENTRY AREA NON-DISPLAY UNTIL  *   FILE 325
//*                      PASSWD GIVEN                               *   FILE 325
//*                    - MAKE "OPER REDISPLAY" AREA MODIFIABLE FOR  *   FILE 325
//*                      REENTRY                                    *   FILE 325
//*                    - FILL BOTH ENTRY AREAS WITH NULLS TO ALLOW  *   FILE 325
//*                      CHAR INSERT                                *   FILE 325
//*                    - REDISPLAY LAST CMD ENTERED BY USER         *   FILE 325
//*                      (INSTEAD OF OPER)                          *   FILE 325
//*                    - MOVE CONSOLE STATUS TABLE TO SEPARATE      *   FILE 325
//*                      CSECT                                      *   FILE 325
//*                    - RESTRUCTURE THE HELP SCREEN AND USE        *   FILE 325
//*                      UPPER/LOWER CASE                           *   FILE 325
//*                    - PROVIDE TSO HELP ENTRY AS COMMENTS AT END  *   FILE 325
//*                      OF SOURCE                                  *   FILE 325
//*                                                                 *   FILE 325
//*         PRINTOFF - THE WIDELY MODIFIED IPO SUPPLIED TSO         *   FILE 325
//*                    COMMAND TO PRINT A DATASET, WITH YET MORE    *   FILE 325
//*                    FUNCTIONS ADDED.  AFTER RESEARCHING ALL      *   FILE 325
//*                    VERSIONS ON THE CBT TAPE, WELLS FARGO        *   FILE 325
//*                    CREATED THIS VERSION FROM SOURCE FROM FOUR   *   FILE 325
//*                    SEPARATE FILES.  WE BELIEVE THIS CONTAINS    *   FILE 325
//*                    ALL FEATURES EXCEPT ONE WHICH WILL BE        *   FILE 325
//*                    ADDED WITH OUR NEXT UPDATE (HONOR EXISTING   *   FILE 325
//*                    CC EVEN IF DCB SAYS "NO CC").                *   FILE 325
//*                                                                 *   FILE 325
//*                    FIXED FOR Y2K BY SAM GOLOB.                  *   FILE 325
//*                                                                 *   FILE 325
//*                    FOLLOWING FIXES WERE FROM SAM LEPORE.        *   FILE 325
//*                                                                 *   FILE 325
//*                    R2 - * CORRECT ERROR, 'NOHEAD' CAUSED BLANK  *   FILE 325
//*                           FIRST PAGE.                           *   FILE 325
//*                         * CORRECT ERRORS IN LENGTH OF TEXTG     *   FILE 325
//*                           THROUGH TEXTJ.                        *   FILE 325
//*                         * INCREASE INPUT RECORD LIMIT TO        *   FILE 325
//*                           32,760.                               *   FILE 325
//*                         * CHANGE DSNAME POSIT TO DSTHING TO     *   FILE 325
//*                           ALLOW FOR DDN().                      *   FILE 325
//*                         * CHANGE DEST KEYWORD TO ACCEPT 8       *   FILE 325
//*                           CHARACTER VALUE.                      *   FILE 325
//*                         * CHANGE DEST KEYWORD TO ACCEPT NODE    *   FILE 325
//*                           AND USERID.                           *   FILE 325
//*                         * ADD DDNAME(...) KEYWORD TO ALLOW TEMP *   FILE 325
//*                           OR VIO DATASETS.                      *   FILE 325
//*                         * ADD UNIT(...) KEYWORD FOR USE WITH    *   FILE 325
//*                           VOLUME(...).                          *   FILE 325
//*                         * ADD 'VOLUME: VOLSER' TO HEADING WHEN  *   FILE 325
//*                           SPECIFIED.                            *   FILE 325
//*              +--------> * ADD TIME AND 'MONTHNAME DAY, YEAR' TO *   FILE 325
//*              :            DSN HEADING.                          *   FILE 325
//*              :          * ADD NOMSGS KEYWORD TO STOP NON-ERROR  *   FILE 325
//*              :            MSGS TO TERMINAL.                     *   FILE 325
//*              :          * ADD DSECT=YES TO CVT MACRO FOR CLEAN  *   FILE 325
//*              :            XA ASSEMBLY.                          *   FILE 325
//*              :          * MOVE ALL PUTLINE TEXT TO SEPARATE     *   FILE 325
//*              :            MESSAGES CSECT.                       *   FILE 325
//*              :            --- (FOLLOWING CHANGES ARE WFB        *   FILE 325
//*              :                SPECIFIC) ---                     *   FILE 325
//*              :          * MAKE WFB DEFAULT FORM($TST) IN PARSE  *   FILE 325
//*              :            MACRO.                                *   FILE 325
//*              :                                                  *   FILE 325
//*            +-- NOTE ==> THE TIME-DATE ROUTINE IKJEFLPA          *   FILE 325
//*            (SEE BELOW)  NORMALLY RESIDES ONLY IN SYS1.AOST4.    *   FILE 325
//*                         THIS LIBRARY MUST BE INCLUDED IN THE    *   FILE 325
//*                         LINKEDIT SYSLIB FOR PROPER              *   FILE 325
//*                         RESOLUTION.                             *   FILE 325
//*                                                                 *   FILE 325
//*            +-- NOTE ==> (SBG - NO MORE. IKJEFLPA FROM IBM IS    *   FILE 325
//*                          NOW INCOMPATIBLE, SO WE DISASSEMBLED   *   FILE 325
//*                          THE OLD ONE, FIXED THE "CENTURY"       *   FILE 325
//*                          PROBLEM, AND WE ARE INCLUDING ITS      *   FILE 325
//*                          SOURCE HERE. ALSO IKJEFLPB.)           *   FILE 325
//*                                                                 *   FILE 325
//*         ROOM     - (THIS VERSION IS UPDATED TO JES2 SP2.1.5     *   FILE 325
//*                    LEVEL.)  A TSO COMMAND TO ALLOW A USER TO    *   FILE 325
//*                    CHANGE THE "ROOM NUMBER" FIELD IN THE JES    *   FILE 325
//*                    JCT FOR THE TSO SESSION.  THIS COMMAND IS    *   FILE 325
//*                    NECESSARY BECAUSE THE ROOM NUMBER FIELD IS   *   FILE 325
//*                    NOT SUPPORTED BY UADS (AND THE FIELD IS      *   FILE 325
//*                    OVERLAYED BY WELLS FARGO ACCOUNTING          *   FILE 325
//*                    INFORMATION DURING LOGON).  ROOM MAKES IT    *   FILE 325
//*                    EASY FOR THE USER TO SPECIFY DELIVERY        *   FILE 325
//*                    INFORMATION (PRINTED ON JES HEADER AND       *   FILE 325
//*                    TRAILER PAGES) FOR ALL SYSOUT CREATED DURING *   FILE 325
//*                    THE SESSION, INCLUDING SPUN DATASETS.        *   FILE 325
//*                                                                 *   FILE 325
//*                    THIS CODE CAN SERVE AS A MODEL FOR ALLOWING  *   FILE 325
//*                    A TSO USER TO CHANGE THROUGH AUTHORIZED      *   FILE 325
//*                    MEANS ANY OTHERWISE PROTECTED INFORMATION IN *   FILE 325
//*                    THE JES JCT OR SIMILAR CONTROL BLOCKS.       *   FILE 325
//*                                                                 *   FILE 325
//*         VTAMCHK  - THIS PROGRAM IS INTENDED TO BE STARTED       *   FILE 325
//*                    AUTOMATICALLY AFTER AN IPL (BY COMMNDXX).    *   FILE 325
//*                    IT IS USED TO START VTAM APPLICATIONS OR     *   FILE 325
//*                    ISSUE OTHER COMMANDS IN AN ORDERLY SEQUENCE  *   FILE 325
//*                    AFTER VTAM IS UP AND RUNNING.  VTAMCHK HAS   *   FILE 325
//*                    THE OPTION TO DELAY BETWEEN ISSUING EACH     *   FILE 325
//*                    COMMAND BECAUSE SOME ENVIRONMENTS            *   FILE 325
//*                    THEMSELVES HAVE TIME DEPENDENCIES, SUCH AS   *   FILE 325
//*                    $SLOGON1   (WAIT FOR INITIALIZATION)         *   FILE 325
//*                                                                 *   FILE 325
//*                            $SN,A=XX                             *   FILE 325
//*                                                                 *   FILE 325
//*                    THE SOURCE HAS A SAMPLE OF THE PROCEDURE     *   FILE 325
//*                    AND SOME COMMANDS.                           *   FILE 325
//*                                                                 *   FILE 325
//*         VTOCLIST - A CORRECTED VERSION OF THE GTE VTOCLIST      *   FILE 325
//*                    PROGRAM TAKEN FROM THE CBT TAPE.             *   FILE 325
//*                    CORRECTIONS INCLUDE:                         *   FILE 325
//*                                                                 *   FILE 325
//*                    - PROVIDE SUPPORT FOR 3380 MODEL E DEVICES.  *   FILE 325
//*                    - PROVIDE SUPPORT FOR DF/EF VSAM FILES WHICH *   FILE 325
//*                      ARE ALLOWED TO HAVE MORE THAN 16 EXTENTS.  *   FILE 325
//*                    - TWO CORRECTIONS TO PRINT EXTENT NUMBERS    *   FILE 325
//*                      GREATER THAN 99.                           *   FILE 325
//*                    - A CORRECTION TO PRINT THE "LAST REF DATA"  *   FILE 325
//*                      AND "USE COUNT".                           *   FILE 325
//*                                                                 *   FILE 325
```
