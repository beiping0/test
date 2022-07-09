在 dao 层中遇到一个 sql.ErrNoRows？
答案：应该
代码：
func (o *querySet) One(container interface{}, cols ...string) error {
    o.limit = 1
    num, err := o.orm.alias.DbBaser.ReadBatch(o.orm.db, o, o.mi, o.cond, container, o.orm.alias.TZ, cols)
    if err != nil {
        return err
    }
    if num == 0 {
        return ErrNoRows
    }

    if num > 1 {
        return ErrMultiRows
    }
    return nil
}
