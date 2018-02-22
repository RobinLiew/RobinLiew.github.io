增和删没什么好说的，比较简单。这里主要介绍查的方法。

- hibernate中merge方法和update的区别

		Session sess = sessionFactory.getCurrentSession();
		sess.merge(obj);//obj是需要添加的对象
		//sess.update(obj);
merge的作用是：新new一个对象，如果该对象设置了id，则这个对象就处于脱管状态（游离状态）：
	- a.  当id在数据库中不能找到时，用update的话会报异常，然而用merge的话，就会insert。
	- b.  当id在数据库中能找到的时候，update与merge的执行效果都是更新数据，发出update语句；
	
- 查的方法
	最常见的是get方法，这里不多说。主要介绍条件查询，条件查询通过如下三个类完成：
	- Criteria：代表一次查询
	- Criterion：代表一个查询条件
	- Restrictions：产生查询条件的工具类
	
```
//根据id进行查询
			public T getById(Serializable id) {
		        try {
		            if (null == id) {
		                return null;
		            }else{
			            Session sess = sessionFactory.getCurrentSession();
			            Criteria criteria = sess.createCriteria(TaskPO.class);//创建与TaskPO绑定的查询类
			            criteria.add(Restrictions.eq("id", id));//给查询类中添加查询条件，Restrictions.eq("id", id)返回SimpleExpression 对象，这个类是Criterion接口的实现。
			            List<T> adList = criteria.list();//list方法返回满足条件的所有结果的集合，对于id查询，集合中中只有一个值。
			            if (adList.isEmpty()) {
			                return null;
			            }
			            return adList.get(0);
		            }
		        } catch (Exception e) {
		            throw new DaoRuntimeException("failed to getById " + ",id: " + id);
		        }
		    }
			
			//========================================================================================
			
			//查询所有
			public List<T> getAll() {
		        try {
		            Session sess = sessionFactory.getCurrentSession();
		            Criteria criteria = sess.createCriteria(getEntityClass());
		            List<T> adList = criteria.list();//给查询类不设置查询条件，可查出所有结果的集合。
		            return adList;
		        } catch (Exception e) {    
		            throw new DaoRuntimeException("failed to getAll ");
		        }
		    }

			//========================================================================================
			
			//多条件查询中，一般条件会有很多，而且也不能确定，因此可以将查询条件封装到一个Map＜String, Object＞集合中, 动态的生成查询语句发送给数据库得到查询结果。
			public <T> List<T> queryResultList(Class<T> className, Map<String,Object> varables){        
		        Session session = sessionFactory.getCurrentSession();
		        Transaction transaction = session.beginTransaction();               
		        List<T> valueList = selectStatement(className, varables, session).list();
		        transaction.commit();
		        return valueList;       
		    }
			//========================================================================================
			
			//下面介绍一种更高级的写法，用一个类将查询条件以及查询条件的运算方式封装起来
			//查询条件和查询条件运算方式的封装类
			public class QueryAssist<T extends Object> {
			    /**
			     * 要查询的类属性名称。
			     */
			    private String attriName;
			    /**
			     * 要查询的类属性值。
			     */
			    private T attriValue;
			    /**
			     * 要查询的类属性对应的关系运算。
			     */
			    private EOperatorType operatorType;
			  
			    public enum EOperatorType {
			        /**
			         *等于运算
			         */
			        OPERATOR_EQ,
			        /**
			         *大于等于运算
			         */
			        OPERATOR_GE,
			        /**
			         *大于运算
			         */
			        OPERATOR_GT,
			        /**
			         *小于等于运算
			         */
			        OPERATOR_LE,
			        /**
			         *小于运算
			         */
			        OPERATOR_LT,
			        /**
			         *不等于运算
			         */
			        OPERATOR_NE,
			        /**
			         *匹配查询
			         */
			        OPERATOR_LIKE,
			        /**
			         *等于空
			         */
			        OPERATOR_NULL,
			        /**
			         *不等于空 
			         */
			        OPERATOR_ISNOTNULL,
			        /**
			         *升序
			         */
			        OPERATOR_ORDER_ASC,
			        /**
			         *降序
			         */
			        OPERATOR_ORDER_DESC,
			        /**
			         *无运算
			         */
			        OPERATOR_NO
			    }
				public static <T> QueryAssist<T> custom(String attriName, T attriValue,
						EOperatorType operatorType, boolean checkValue) {
			        QueryAssist<T> queryAssist = new QueryAssist<>();
			        if (null == attriName || null == operatorType) {
			            return null;
			        }
			
			        if (checkValue && null == attriValue) {
			            return null;
			        }
			
			        queryAssist.setAttriName(attriName);
			
			        if (null != attriValue) {
			            queryAssist.setAttriValue(attriValue);
			        }
			
			        queryAssist.setOperatorType(operatorType);
			
			        return queryAssist;
			    }
			    
			    public static <T> QueryAssist<T> createQueryAssist(String name,T value, EOperatorType oprType){
			        QueryAssist<T> queryAssist = new QueryAssist<>();
			        queryAssist.setAttriName(name);
			        if(null != value){
			            queryAssist.setAttriValue(value);
			        }
			        queryAssist.setOperatorType(oprType);
			        
			        return queryAssist;
			    }

			    public String getAttriName() {
			        return attriName;
			    }
			
			    public void setAttriName(String attriName) {
			        this.attriName = attriName;
			    }
			
			    public T getAttriValue() {
			        return attriValue;
			    }
			
			    public void setAttriValue(T attriValue) {
			        this.attriValue = attriValue;
			    }
			
			    public EOperatorType getOperatorType() {
			        return operatorType;
			    }
			
			    public void setOperatorType(EOperatorType operatorType) {
			        this.operatorType = operatorType;
			    }
    
			}

			//========================================================================================

			//接下来使用这个查询封装类进行多条件查询
			protected boolean spliceQuery(Criteria criteria, List<QueryAssist> querylist) {
			        for (int i = 0; i < querylist.size(); i++) {
		            QueryAssist queryassist = querylist.get(i);
		            switch (queryassist.getOperatorType()) {
		                case OPERATOR_EQ:
		                    criteria.add(Restrictions.eq(queryassist.getAttriName(),
		                            queryassist.getAttriValue()));
		                    break;
		                case OPERATOR_GE:
		                    criteria.add(Restrictions.ge(queryassist.getAttriName(),
		                            queryassist.getAttriValue()));
		                    break;
		                case OPERATOR_GT:
		                    criteria.add(Restrictions.gt(queryassist.getAttriName(),
		                            queryassist.getAttriValue()));
		                    break;
		                case OPERATOR_LE:
		                    criteria.add(Restrictions.le(queryassist.getAttriName(),
		                            queryassist.getAttriValue()));
		                    break;
		                case OPERATOR_LT:
		                    criteria.add(Restrictions.lt(queryassist.getAttriName(),
		                            queryassist.getAttriValue()));
		                    break;
		                case OPERATOR_NE:
		                    criteria.add(Restrictions.ne(queryassist.getAttriName(),
		                            queryassist.getAttriValue()));
		                    break;
		                case OPERATOR_LIKE:
		                    String attrValue = (String) queryassist.getAttriValue();
		                    if (!attrValue.startsWith("%") && !attrValue.endsWith("%")) {
		                        criteria.add(Restrictions.like(queryassist.getAttriName(), "%"
		                                + queryassist.getAttriValue() + "%"));
		                    } else {
		                        criteria.add(Restrictions.like(queryassist.getAttriName(),
		                                queryassist.getAttriValue()));
		                    }
		                    break;
		                case OPERATOR_ORDER_ASC:
		                    criteria.addOrder(Order.asc(queryassist.getAttriName()));
		                    break;
		                case OPERATOR_ORDER_DESC:
		                    criteria.addOrder(Order.desc(queryassist.getAttriName()));
		                    break;
		                case OPERATOR_NULL:
		                    criteria.add(Restrictions.isNull(queryassist.getAttriName()));
		                    break;
		                case OPERATOR_ISNOTNULL:
		                    criteria.add(Restrictions.isNotNull(queryassist.getAttriName()));
		                    break;
		                default:
		                    logger.info("there is no operator");
		                    return false;
		            }
		        }
		        return true;
			}
			
			//========================================================================================
			
			//通过属性进行查询
			public List<T> findByAttribute(List<QueryAssist> querylist) {
			        try {
			            if (querylist == null) {
			                return null;
			            }
			            Session sess = sessionFactory.getCurrentSession();
			            Criteria criteria = sess.createCriteria(getEntityClass());
			            boolean result = this.spliceQuery(criteria, querylist);
			            if (!result) {
			                return null;
			            }
			            return criteria.list();
			        } catch (Exception e) {
			          
			            throw new DaoRuntimeException("failed to find  by attribute");
			        }
			}

			//========================================================================================

			//根据上面的查询方法，我们写一下多条件的分页查询
			public List<T> findByPage(List<QueryAssist> querylist, final int offset, final int pageSize) {//offset指定从第几条记录开始查询，pageSize指定一页的记录数量
		        try {
		            if (null == querylist) {
		                logger.warn("null param when findByPage by querylist ");
		                return null;
		            }
		            Session sess = sessionFactory.getCurrentSession();
		            Criteria criteria = sess.createCriteria(TaskPO.class);
		            //创建查询条件
		            this.spliceQuery(criteria, querylist);
		            criteria.setFirstResult(offset).setMaxResults(pageSize);//设置查询起始及范围
		            return criteria.list();
		        } catch (Exception e) {
		            logger.error("failed to findByPage for " + getEntityClass(), e);
		            throw new DaoRuntimeException("failed to findByPage for " + getEntityClass());
		        }
		    }

			//========================================================================================
			
			//分页查询等查询必须要查出对应条件下所有结果的数量
			public long countByAttri(List<QueryAssist> queryAssists) throws DaoRuntimeException {

		        try {
		            Session sess = sessionFactory.getCurrentSession();
		            Criteria criteria = sess.createCriteria(TaskPO.class);
		            this.spliceQuery(criteria, queryAssists);
		           //使用Hibernate的投影统计功能。Projections工厂类提供了几个用来获取Projection实例的静态工厂方法，在获得Projection对象之后，使用setProjection()方法将它添加到Criteria对象中。注意，返回的结果集是Object类型，需要对结果进行适当的类型转换。
		            long rowCount = (long) (criteria.setProjection(Projections.rowCount()).uniqueResult());//所有查询结果的数量
		            criteria.setProjection(null);//使用完毕后将查询类中的Projection对象置为空
		            return rowCount;
		        } catch (Exception e) {
		            throw new DaoRuntimeException("Count  failed: "
	                    + queryAssists);
		        }
		    }			    		 

//=======================================================================
```
			

	