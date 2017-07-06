## 空间字段 SDO_GEOMETRY 查询使用方法
----------------------------------
### 向元数据表(user_sdo_geom_metadata)插入某表(WAYS)的空间字段(POSITION)的约束
    insert into user_sdo_geom_metadata(table_name,COLUMN_NAME,DIMINFO,SRID)
    values(
        'WAYS',
        'POSITION',
        MDSYS.SDO_DIM_ARRAY(
            MDSYS.SDO_DIM_ELEMENT('X',-180,180,0.0000000001),
            MDSYS.SDO_DIM_ELEMENT('Y',-90,90,0.0000000001)
        ),
        NULL
    );
### 查看元数据表
select * from user_sdo_geom_metadata;
--创建空间索引
Create INDEX WAYS_SIDX on WAYS(POSITION) Indextype is MDSYS.SPATIAL_INDEX;
-- 查看表的索引
select * from user_indexes where table_name='WAYS';

--空间字段查询使用
SELECT * FROM
WAYS A
WHERE
SDO_WITHIN_DISTANCE(
    A.POSITION,
    MDSYS.SDO_GEOMETRY(2001,NULL,MDSYS.SDO_POINT_TYPE(58865.7200999931,45831.047199994304,NULL),NULL,NULL),
    'DISTANCE=1' --DISTANCE 的值为坐标单位
) = 'TRUE'

--最近的一个空间位置结果 SDO_NUM_RES = 1
SELECT * FROM
WAYS A
WHERE
SDO_NN(
    A.POSITION,
    MDSYS.SDO_GEOMETRY(2001,NULL,MDSYS.SDO_POINT_TYPE(58864.7200999931,45832.047199994304,NULL),NULL,NULL),'SDO_NUM_RES=1',1
) = 'TRUE'


--查找事故所属路段
create TABLE SG_LD AS(SELECT A.LINK_ID,C.ID FROM
TMP A,
(SELECT * FROM TMP_SG
    WHERE ID IN (SELECT B.ID FROM TMP_SG B,TMP A
        WHERE
        SDO_WITHIN_DISTANCE(
            B.SHAPE,
            A.SHAPE,
            'DISTANCE=0.002' --DISTANCE 的值为坐标单位
        ) = 'TRUE')
) C
WHERE
SDO_NN(
    A.SHAPE,
    C.SHAPE ,'SDO_NUM_RES=1',1
) = 'TRUE')


-- 空间字段应用
insert into user_sdo_geom_metadata(table_name,COLUMN_NAME,DIMINFO,SRID)
values(
    'TMP',
    'SHAPE',
    MDSYS.SDO_DIM_ARRAY(
        MDSYS.SDO_DIM_ELEMENT('X',-180,180,0.00000001),
        MDSYS.SDO_DIM_ELEMENT('Y',-90,90,0.00000001)
    ),
    NULL
);
select * from user_sdo_geom_metadata;
Create INDEX TMP_SG_SIDX on TMP_SG(SHAPE) Indextype is MDSYS.SPATIAL_INDEX
select * from user_indexes where table_name='TMP_SG';

SELECT * FROM
TMP A
WHERE
SDO_NN(
    A.SHAPE,
    MDSYS.SDO_GEOMETRY(2001,NULL,MDSYS.SDO_POINT_TYPE(120.726830,31.306243,NULL),NULL,NULL),
    'SDO_NUM_RES=1',1
) = 'TRUE'

SELECT LINK_ID,A.SHAPE.SDO_ORDINATES FROM
TMP A
WHERE
SDO_WITHIN_DISTANCE(
    A.SHAPE,
    MDSYS.SDO_GEOMETRY(2001,NULL,MDSYS.SDO_POINT_TYPE(120.726830,31.306243,NULL),NULL,NULL),
    'DISTANCE=0.001'
