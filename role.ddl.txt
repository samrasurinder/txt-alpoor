set long 100000
set longchunksize 200
set heading off
set feedback off
set echo off
set verify off
undefine role

select (case
when ((select count(*)
from dba_roles
where role = '&&role') > 0)
then dbms_metadata.get_ddl ('ROLE', '&&role')
else to_clob ('Role does not exist')
end ) Extracted_DDL from dual
UNION ALL
select (case
when ((select count(*)
from dba_role_privs
where grantee = '&&role') > 0)
then dbms_metadata.get_granted_ddl ('ROLE_GRANT', '&&role')
end ) from dual
UNION ALL
select (case
when ((select count(*)
from dba_role_privs
where grantee = '&&role') > 0)
then dbms_metadata.get_granted_ddl ('DEFAULT_ROLE', '&&role')
end ) from dual
UNION ALL
select (case
when ((select count(*)
from dba_sys_privs
where grantee = '&&role') > 0)
then dbms_metadata.get_granted_ddl ('SYSTEM_GRANT', '&&role')
end ) from dual
UNION ALL
select (case
when ((select count(*)
from dba_tab_privs
where grantee = '&&role') > 0)
then dbms_metadata.get_granted_ddl ('OBJECT_GRANT', '&&role')
end ) from dual;