1. db的事务隔离级别（1m）
    acid，
    原子性： undo log（mvcc）
    一致性：最核心最本质的要求
    隔离性：mvcc
    持久性：redo log
1. mvcc的实现
    1. 当前读：当前数据。select for update； insert
    1. 快照读：历史版本数据。select
    1. mvcc由3个隐式字段、undo日志、read view三个组件实现