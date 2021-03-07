# mysql_sys.x-schema_flattened_keys-

SELECT 
    `information_schema`.`statistics`.`TABLE_SCHEMA` AS `TABLE_SCHEMA`,
    `information_schema`.`statistics`.`TABLE_NAME` AS `TABLE_NAME`,
    `information_schema`.`statistics`.`INDEX_NAME` AS `INDEX_NAME`,
    MAX(`information_schema`.`statistics`.`NON_UNIQUE`) AS `non_unique`,
    MAX(IF((`information_schema`.`statistics`.`SUB_PART` IS NULL),
        0,
        1)) AS `subpart_exists`,
    GROUP_CONCAT(`information_schema`.`statistics`.`COLUMN_NAME`
        ORDER BY `information_schema`.`statistics`.`SEQ_IN_INDEX` ASC
        SEPARATOR ',') AS `index_columns`
FROM
    `information_schema`.`STATISTICS`
WHERE
    ((`information_schema`.`statistics`.`INDEX_TYPE` = 'BTREE')
        AND (`information_schema`.`statistics`.`TABLE_SCHEMA` NOT IN ('mysql' , 'sys',
        'INFORMATION_SCHEMA',
        'PERFORMANCE_SCHEMA')))
GROUP BY `information_schema`.`statistics`.`TABLE_SCHEMA` , `information_schema`.`statistics`.`TABLE_NAME` , `information_schema`.`statistics`.`INDEX_NAME`
