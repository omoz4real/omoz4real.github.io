---
layout: post
title: "Displaying and Using PrimeFaces Bar chart From a Database"
date: 2021-05-21
---
This blog post is about displaying a bar chart from a database in a Jakarta Faces application using PrimeFaces. In the [PrimeFaces user documentation](https://www.primefaces.org/showcase/ui/chartjs/bar/bar.xhtml?jfwid=929d7), the examples use static data to produce different charts using hard coded values. There is already a CRUD Jakarta EE application that have been developed using Jakarta Faces + Jakarta Enterprise Beans + Jakarta Persistence and MYSQL as the Database. The data to be visually represented and rendered in the application using the PrimeFaces Bar Chart component is to be read from this database.

The Entity Class for the application looks like this

```
package lifesavers.database.entity;

import java.io.Serializable;
import java.math.BigDecimal;
import java.text.SimpleDateFormat;
import java.util.Date;
import javax.persistence.Basic;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.NamedQueries;
import javax.persistence.NamedQuery;
import javax.persistence.Table;
import javax.persistence.Temporal;
import javax.persistence.TemporalType;
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Size;
//import javax.xml.bind.annotation.XmlRootElement;

/**
 *
 * @author omozegieaziegbe
 */
@Entity
@Table(name = "customer", catalog = "lifesaverscanada", schema = "")
//@XmlRootElement
@NamedQueries({
@NamedQuery(name = "Customer.findAll", query = "SELECT c FROM Customer c")
, @NamedQuery(name = "Customer.findById", query = "SELECT c FROM Customer c WHERE c.id = :id")
, @NamedQuery(name = "Customer.findByFirstname", query = "SELECT c FROM Customer c WHERE c.firstname = :firstname")
, @NamedQuery(name = "Customer.findByMiddlename", query = "SELECT c FROM Customer c WHERE c.middlename = :middlename")
, @NamedQuery(name = "Customer.findByLastname", query = "SELECT c FROM Customer c WHERE c.lastname = :lastname")
, @NamedQuery(name = "Customer.findByPrimaryEmail", query = "SELECT c FROM Customer c WHERE c.primaryEmail = :primaryEmail")
, @NamedQuery(name = "Customer.findBySecondaryEmail", query = "SELECT c FROM Customer c WHERE c.secondaryEmail = :secondaryEmail")
, @NamedQuery(name = "Customer.findByPhone", query = "SELECT c FROM Customer c WHERE c.phone = :phone")
, @NamedQuery(name = "Customer.findByAddress", query = "SELECT c FROM Customer c WHERE c.address = :address")
, @NamedQuery(name = "Customer.findByCity", query = "SELECT c FROM Customer c WHERE c.city = :city")
, @NamedQuery(name = "Customer.findByProvince", query = "SELECT c FROM Customer c WHERE c.province = :province")
, @NamedQuery(name = "Customer.findByPostalCode", query = "SELECT c FROM Customer c WHERE c.postalCode = :postalCode")
, @NamedQuery(name = "Customer.findByCompanyName", query = "SELECT c FROM Customer c WHERE c.companyName = :companyName")
, @NamedQuery(name = "Customer.findByNotes", query = "SELECT c FROM Customer c WHERE c.notes = :notes")
, @NamedQuery(name = "Customer.findByDob", query = "SELECT c FROM Customer c WHERE c.dob = :dob")
, @NamedQuery(name = "Customer.findByCourseNumber", query = "SELECT c FROM Customer c WHERE c.courseNumber = :courseNumber")
, @NamedQuery(name = "Customer.findByCourseDate", query = "SELECT c FROM Customer c WHERE c.courseDate = :courseDate")
, @NamedQuery(name = "Customer.findByCourseName", query = "SELECT c FROM Customer c WHERE c.courseName = :courseName")
, @NamedQuery(name = "Customer.findByCprLevel", query = "SELECT c FROM Customer c WHERE c.cprLevel = :cprLevel")
, @NamedQuery(name = "Customer.findByTrainingLocation", query = "SELECT c FROM Customer c WHERE c.trainingLocation = :trainingLocation")
, @NamedQuery(name = "Customer.findByCourseTime", query = "SELECT c FROM Customer c WHERE c.courseTime = :courseTime")
, @NamedQuery(name = "Customer.findByCurrentCardExpDate", query = "SELECT c FROM Customer c WHERE c.currentCardExpDate = :currentCardExpDate")
, @NamedQuery(name = "Customer.findByGrade", query = "SELECT c FROM Customer c WHERE c.grade = :grade")
, @NamedQuery(name = "Customer.findByStatus", query = "SELECT c FROM Customer c WHERE c.status = :status")
, @NamedQuery(name = "Customer.findByInstructorName1", query = "SELECT c FROM Customer c WHERE c.instructorName1 = :instructorName1")
, @NamedQuery(name = "Customer.findByInstructorName2", query = "SELECT c FROM Customer c WHERE c.instructorName2 = :instructorName2")
, @NamedQuery(name = "Customer.findByNeedCard", query = "SELECT c FROM Customer c WHERE c.needCard = :needCard")
, @NamedQuery(name = "Customer.findByCrchsfRoster", query = "SELECT c FROM Customer c WHERE c.crchsfRoster = :crchsfRoster")
, @NamedQuery(name = "Customer.findByCrchsfSubmitted", query = "SELECT c FROM Customer c WHERE c.crchsfSubmitted = :crchsfSubmitted")
, @NamedQuery(name = "Customer.findByQbSubmitted", query = "SELECT c FROM Customer c WHERE c.qbSubmitted = :qbSubmitted")
, @NamedQuery(name = "Customer.findByPromoCode", query = "SELECT c FROM Customer c WHERE c.promoCode = :promoCode")
, @NamedQuery(name = "Customer.findByPrice", query = "SELECT c FROM Customer c WHERE c.price = :price")
, @NamedQuery(name = "Customer.findByDiscount", query = "SELECT c FROM Customer c WHERE c.discount = :discount")
, @NamedQuery(name = "Customer.findByDiscountSaved", query = "SELECT c FROM Customer c WHERE c.discountSaved = :discountSaved")
, @NamedQuery(name = "Customer.findByGst", query = "SELECT c FROM Customer c WHERE c.gst = :gst")
, @NamedQuery(name = "Customer.findByPurchasedMask", query = "SELECT c FROM Customer c WHERE c.purchasedMask = :purchasedMask")
, @NamedQuery(name = "Customer.findByTotalWithGst", query = "SELECT c FROM Customer c WHERE c.totalWithGst = :totalWithGst")
, @NamedQuery(name = "Customer.findByPaymentType", query = "SELECT c FROM Customer c WHERE c.paymentType = :paymentType")})
public class Customer implements Serializable {
    
    private static final long serialVersionUID = 1L;
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Basic(optional = false)
    @Column(name = "id", nullable = false)
    private Integer id;
    @Basic(optional = false)
    @NotNull
    @Size(min = 1, max = 245)
    @Column(name = "firstname", length = 245)
    private String firstname;
    
    @Basic(optional = true)
    @Column(name = "middlename", length = 245)
    private String middlename;
    
    @Basic(optional = true)
    @NotNull
    @Size(min = 1, max = 245)
    @Column(name = "lastname", length = 245)
    private String lastname;
    
    @Basic(optional = true)
    @NotNull
    @Size(min = 1, max = 245)
    @Column(name = "primary_email", length = 245)
    private String primaryEmail;
    @Size(max = 45)
    @Column(name = "secondary_email", length = 45)
    private String secondaryEmail;
    // @Pattern(regexp="^\\(?(\\d{3})\\)?[- ]?(\\d{3})[- ]?(\\d{4})$", message="Invalid phone/fax format, should be as xxx-xxx-xxxx")//if the field contains phone or fax number consider using this annotation to enforce field validation
    @Basic(optional = true)
    @NotNull
    @Size(min = 1, max = 45)
    @Column(name = "phone", length = 45)
    private String phone;
    @Basic(optional = true)
    //@NotNull
    //@Size(min = 1, max = 245)
    @Column(name = "address", length = 245)
    private String address;
    @Size(max = 45)
    @Column(name = "city", length = 45)
    private String city;
    @Size(max = 45)
    @Column(name = "province", length = 45)
    private String province;
    @Basic(optional = true)
    //@NotNull
    //@Size(min = 1, max = 45)
    @Column(name = "postal_code", length = 45)
    private String postalCode;
    @Size(max = 245)
    @Column(name = "company_name", length = 245)
    private String companyName;
    @Size(max = 245)
    @Column(name = "notes", length = 245)
    private String notes;
    @Basic(optional = false)
    @Column(name = "dob")
    @Temporal(TemporalType.DATE)
    private Date dob;
    @Size(max = 45)
    @Column(name = "course_number", length = 45)
    private String courseNumber;
    @Column(name = "course_date")
    @Temporal(TemporalType.DATE)
    private Date courseDate;
    @Size(max = 45)
    @Column(name = "course_name", length = 45)
    private String courseName;
    @Size(max = 45)
    @Column(name = "cpr_level", length = 45)
    private String cprLevel;
    @Size(max = 45)
    @Column(name = "training_location", length = 45)
    private String trainingLocation;
    @Size(max = 245)
    @Column(name = "course_time", length = 245)
    private String courseTime;
    @Size(max = 45)
    @Column(name = "current_card_exp_date", length = 45)
    private String currentCardExpDate;
    @Size(max = 45)
    @Column(name = "grade", length = 45)
    private String grade;
    @Size(max = 45)
    @Column(name = "status", length = 45)
    private String status;
    @Size(max = 245)
    @Column(name = "instructor_name_1", length = 245)
    private String instructorName1;
    @Size(max = 245)
    @Column(name = "instructor_name_2", length = 245)
    private String instructorName2;
    @Column(name = "need_card")
    private Boolean needCard;
    @Column(name = "crchsfRoster")
    private Boolean crchsfRoster;
    @Column(name = "crchsfSubmitted")
    private Boolean crchsfSubmitted;
    @Column(name = "qbSubmitted")
    private Boolean qbSubmitted;
    @Size(max = 45)
    @Column(name = "promo_code", length = 45)
    private String promoCode;
    // @Max(value=?)  @Min(value=?)//if you know range of your decimal fields consider using these annotations to enforce field validation
    @Column(name = "price", precision = 8, scale = 2)
    private BigDecimal price;
    @Column(name = "discount")
    private Integer discount;
    @Column(name = "discount_saved", precision = 8, scale = 2)
    private BigDecimal discountSaved;
    @Column(name = "purchased_mask", precision = 8, scale = 2)
    private BigDecimal purchasedMask;
    @Column(name = "gst", precision = 8, scale = 2)
    private BigDecimal gst;
    @Column(name = "total_with_gst", precision = 8, scale = 2)
    private BigDecimal totalWithGst;
    @Size(max = 245)
    @Column(name = "payment_type", length = 245)
    private String paymentType;
    
    
    public Customer() {
    }
    
    public Customer(Integer id) {
        this.id = id;
    }
    
    public Customer(Integer id, String firstname, String middlename, String lastname, String primaryEmail, String phone, String address, String postalCode) {
        this.id = id;
        this.firstname = firstname;
        this.middlename = middlename;
        this.lastname = lastname;
        this.primaryEmail = primaryEmail;
        this.phone = phone;
        this.address = address;
        this.postalCode = postalCode;
    }
    
// Note that getters and setters, hashCode, equals and toString method have been ommitted
    
}
```

The EJB Abstract Facade for the application looks like this

```
package lifesavers.database.session;

import java.math.BigDecimal;
import java.util.Arrays;
import java.util.Date;
import java.util.List;
import javax.persistence.EntityManager;
import javax.persistence.Query;
import javax.persistence.TemporalType;
import lifesavers.database.entity.Customer;

/**
 *
 * @author omozegieaziegbe
 */
public abstract class AbstractFacade<T> {

    private Class<T> entityClass;

    public AbstractFacade(Class<T> entityClass) {
        this.entityClass = entityClass;
    }

    protected abstract EntityManager getEntityManager();

    public void create(T entity) {
        getEntityManager().persist(entity);
    }

    public void edit(T entity) {
        getEntityManager().merge(entity);
    }

    public void remove(T entity) {
        getEntityManager().remove(getEntityManager().merge(entity));
    }

    public T find(Object id) {
        return getEntityManager().find(entityClass, id);
    }

    public List<T> findAll() {
        javax.persistence.criteria.CriteriaQuery cq = getEntityManager().getCriteriaBuilder().createQuery();
        cq.select(cq.from(entityClass));
        return getEntityManager().createQuery(cq).getResultList();
    }

    public List<T> findRange(int[] range) {
        javax.persistence.criteria.CriteriaQuery cq = getEntityManager().getCriteriaBuilder().createQuery();
        cq.select(cq.from(entityClass));
        javax.persistence.Query q = getEntityManager().createQuery(cq);
        q.setMaxResults(range[1] - range[0] + 1);
        q.setFirstResult(range[0]);
        return q.getResultList();
    }

    public int count() {
        javax.persistence.criteria.CriteriaQuery cq = getEntityManager().getCriteriaBuilder().createQuery();
        javax.persistence.criteria.Root<T> rt = cq.from(entityClass);
        cq.select(getEntityManager().getCriteriaBuilder().count(rt));
        javax.persistence.Query q = getEntityManager().createQuery(cq);
        return ((Long) q.getSingleResult()).intValue();
    }


    public List<String> findCustomersForChart() {
        Query query = getEntityManager().createQuery("SELECT DISTINCT (c.courseName) FROM Customer c WHERE c.courseName is not NULL ORDER BY c.courseName ASC");
        List<String> chartList = query.getResultList();
        return chartList;
    }

    public List<BigDecimal> findCustomersForBarChart() {
        Query query = getEntityManager().createQuery("Select SUM (c.totalWithGst) from Customer c WHERE c.courseName is not NULL GROUP BY c.courseName ORDER BY c.courseName ASC");
        List<BigDecimal> chartList = query.getResultList();
        return chartList;
    }
}

```

The EJB Customer Facade looks like this

```
package lifesavers.database.session;

import javax.ejb.Stateless;
import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import lifesavers.database.entity.Customer;

/**
 *
 * @author omozegieaziegbe
 */
@Stateless
public class CustomerFacade extends AbstractFacade<Customer> {

    @PersistenceContext(unitName = "omos.microsystems_lifesaverscanada_war_1.0-SNAPSHOTPU")
    private EntityManager em;

    @Override
    protected EntityManager getEntityManager() {
        return em;
    }

    public CustomerFacade() {
        super(Customer.class);
    }
    
}

```

And the Bar Chart bean (ChartBean) for the application looks like this

```
package lifesavers.database.clientbean;

import javax.inject.Named;
import java.io.Serializable;
import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.List;
import javax.annotation.PostConstruct;
import javax.ejb.EJB;
import javax.enterprise.context.RequestScoped;
import lifesavers.database.session.CustomerFacade;
import org.primefaces.model.chart.ChartSeries;
import org.primefaces.model.charts.ChartData;
import org.primefaces.model.charts.axes.cartesian.CartesianScales;
import org.primefaces.model.charts.axes.cartesian.linear.CartesianLinearAxes;
import org.primefaces.model.charts.axes.cartesian.linear.CartesianLinearTicks;
import org.primefaces.model.charts.bar.BarChartDataSet;
import org.primefaces.model.charts.bar.BarChartModel;
import org.primefaces.model.charts.bar.BarChartOptions;
import org.primefaces.model.charts.optionconfig.animation.Animation;
import org.primefaces.model.charts.optionconfig.legend.Legend;
import org.primefaces.model.charts.optionconfig.legend.LegendLabel;
import org.primefaces.model.charts.optionconfig.title.Title;

/**
 *
 * @author omozegieaziegbe
 */
@Named(value = "chartBean")
@RequestScoped
public class ChartBean implements Serializable {


    private BarChartModel barModel;
    @EJB
    private CustomerFacade customerfacade;
    
    @PostConstruct
    public void init() {
    createCustomerBarModel();
    }
    /**
     * Creates a new instance of ChartBean
     */
    public ChartBean() {
    }
    
        public void createCustomerBarModel() {
        ChartSeries customerseries = new ChartSeries();
        List<BigDecimal> customerList = customerfacade.findCustomersForBarChart();
        List<String> customerChart = customerfacade.findCustomersForChart();

        List<Number> customerMap = new ArrayList<>();
        List<String> labels = new ArrayList<>();

        for (Number o : customerList) {
            customerMap.add(o);
        }
        for (String a : customerChart) {
            labels.add(a);
        }

        barModel = new BarChartModel();
        ChartData data = new ChartData();
        BarChartDataSet barDataSet = new BarChartDataSet();

        barDataSet.setLabel("Company Name Dataset");
        barDataSet.setData(customerMap);

        List<String> bgColor = new ArrayList<>();
        bgColor.add("rgba(255, 99, 132, 0.2)");
        bgColor.add("rgba(255, 159, 64, 0.2)");
        bgColor.add("rgba(255, 205, 86, 0.2)");
        bgColor.add("rgba(75, 192, 192, 0.2)");
        bgColor.add("rgba(54, 162, 235, 0.2)");
        bgColor.add("rgba(153, 102, 255, 0.2)");
        bgColor.add("rgba(201, 203, 207, 0.2)");
        bgColor.add("rgb(41, 162, 190)");
        bgColor.add("rgb(5, 205, 68)");
        bgColor.add("rgb(135, 109, 92)");
        bgColor.add("rgb(14, 162, 25)");
        bgColor.add("rgb(175, 205, 83)");
        bgColor.add("rgb(185, 199, 172)");
        bgColor.add("rgb(54, 162, 235)");
        bgColor.add("rgb(195, 105, 196)");
        bgColor.add("rgb(245, 99, 132)");
        bgColor.add("rgb(184, 162, 100)");
        bgColor.add("rgb(255, 205, 86)");
        bgColor.add("rgb(215, 199, 112)");
        bgColor.add("rgb(104, 102, 111)");
        bgColor.add("rgb(155, 105, 186)");
        barDataSet.setBackgroundColor(bgColor);

        List<String> borderColor = new ArrayList<>();
        borderColor.add("rgb(255, 99, 132)");
        borderColor.add("rgb(255, 159, 64)");
        borderColor.add("rgb(255, 205, 86)");
        borderColor.add("rgb(75, 192, 192)");
        borderColor.add("rgb(54, 162, 235)");
        borderColor.add("rgb(153, 102, 255)");
        borderColor.add("rgb(201, 203, 207)");
        barDataSet.setBorderColor(borderColor);
        barDataSet.setBorderWidth(1);

        data.addChartDataSet(barDataSet);
        data.setLabels(labels);
        barModel.setData(data);
        barModel.setExtender("extender1");
        
        BarChartOptions options = new BarChartOptions();
        CartesianScales cScales = new CartesianScales();
        CartesianLinearAxes linearAxes = new CartesianLinearAxes();
        linearAxes.setOffset(true);
        CartesianLinearTicks ticks = new CartesianLinearTicks();
        ticks.setBeginAtZero(true);
        linearAxes.setTicks(ticks);
        cScales.addYAxesData(linearAxes);
        options.setScales(cScales);

        Title title = new Title();
        title.setDisplay(true);
        title.setFontSize(20);
        title.setText("Revenue generated grouped by course name");
        options.setTitle(title);

        Legend legend = new Legend();
        legend.setDisplay(true);
        legend.setPosition("top");
        LegendLabel legendLabels = new LegendLabel();
        legendLabels.setFontStyle("bold");
        legendLabels.setFontColor("#9370DB");
        legendLabels.setFontSize(25);
        legend.setLabels(legendLabels);
        options.setLegend(legend);

        // disable animation
        Animation animation = new Animation();
        animation.setDuration(0);
        options.setAnimation(animation);

        barModel.setOptions(options);
    }

    public BarChartModel getBarModel() {
        return barModel;
    }

    public void setBarModel(BarChartModel barModel) {
        this.barModel = barModel;
    }
        }

```
Finally the JSF (Faces) page to display the bar chart component looks like this

```
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:h="http://xmlns.jcp.org/jsf/html"
      xmlns:f="http://java.sun.com/jsf/core"
      xmlns:ui="http://java.sun.com/jsf/facelets"
      xmlns:p="http://primefaces.org/ui">
    <h:head>
        <title>Company BarChart</title>
        <h:outputStylesheet name="css/lifesavers.css"/>
        <h:outputScript library="javax.faces" name="jsf.js" target="body" />
        <script>
            function extender1() {
                var options = {
                    options: {
                        scales: {
                            yAxes: [{
                                    scaleLabel: {
                                        display: true,
                                        labelString: 'Total Amount With GST'
                                    }
                                }],
                        }
                    }
                };
                $.extend(true, this.cfg.config, options);
            }
            ;

        </script>
    </h:head>
    <h:body>
        <h:form>
            <header>
                <nav class="clear">                 
                </nav>
            </header>

            <div class="card" style="border: 0px; height: 800px; width:85%; margin-left: 15px">
                <p:barChart model="#{chartBean.barModel}"   />
            </div>
        </h:form>

        <footer>
            <p>Copyright 2021.</p>
        </footer>
    </h:body>
</html>

```

When the application is completed and built, the Bar Chart should look like the figure below:

![PrimeFaces Barchart](https://omoz4real.github.io/img/icons/barchart.png)

Note: The Primefaces version used in this application is 10.0.0 and the application code for primefaces sometimes changes.
