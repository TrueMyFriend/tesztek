node_modules/bootstrap/dist/css/bootstrap.min.css

koynv controller html

    <div class="container">
    <h1 class="text-center my-4" >Könyv Bolt</h1>
    <input type="text" class="form-controll mb-3" placeholder="Keresés..." [(ngModel)]="kereses" />
    <table class="table table-bordered" >
        <thead class="table-dark" >
            <tr>    
                <th>ISBN</th>
                <th>Cím</th>
                <th>Szerző</th>
                <th>Ár (Ft)</th>
            </tr>
        </thead>
        <tbody>
            <tr *ngFor="let konyv of szurtKonyvek()" [ngClass]="{'olcso':konyv.ar<=3000, 'draga':konyv.ar>3000}" >
                <td>{{konyv.ISBN}}</td>
                <td>{{konyv.cim}}</td>
                <td>{{konyv.szerzok}}</td>
                <td>{{konyv.ar}}</td>
            </tr>
        </tbody>
    </table>
    </div>



konyv controller css

tr.olcso td{
    background-color: red;
}
tr.draga td{
    background-color: blue;
}



konyv controller ts

@Component({
  selector: 'app-konyv-controller',
  templateUrl: './konyv-controller.component.html',
  styleUrls: ['./konyv-controller.component.css']
})
export class KonyvControllerComponent implements OnInit {
  konyvek:any[]=[];
  kereses:string='';
  constructor(private http:HttpClient){}
  ngOnInit(): void {
    this.http.get<any[]>('/assets/konyvek.json').subscribe({
      next:(data)=>{
        this.konyvek=data.map(konyv=>({...konyv, ar: Number(konyv.ar)}));
        console.log('Adatok betöltve:',this.konyvek);
      },
      error:(err)=>console.error('Hiba az adatok betöltésekor:',err)
    });
 
    }
   
      szurtKonyvek():any[]{
      return this.konyvek.filter(konyv=>
      konyv.cim.toLowerCase().includes(this.kereses.toLowerCase())
      );

      }
  }


C CAPA GUI
(fentre)
 OpenFileDialog ofd;
 List<Utazas> tabla = new List<Utazas>();

 megnyitastoolstrip
             //4
            ofd = new OpenFileDialog();
            if (ofd.ShowDialog() == DialogResult.OK) { 
            StreamReader olvaso = new StreamReader(ofd.FileName);
                olvaso.ReadLine();
                while (!olvaso.EndOfStream)
                {
                    tabla.Add(new Utazas(olvaso.ReadLine()));
                }
                olvaso.Close();
            }

            for (int i = 0; i < tabla.Count; i++)
            {
                listBox1.Items.Add($"{tabla[i].utiCel} - {tabla[i].indulasDatum} ");
            }

             var csoportositott = tabla.GroupBy(x => x.utazasMod);
            foreach (var csoport in csoportositott)
            {
                comboBox1.Items.Add(csoport.Key);
            }


//5

listbox1 selected indexchnage

            int index = listBox1.SelectedIndex;
            textBox1.Text = $"{tabla[index].napok}";
            textBox2.Text = $"{tabla[index].ar}";
            textBox3.Text = $"{tabla[index].szallasNeve}";
            textBox4.Text = $"{tabla[index].utazasMod}";


legjobb ajalnat gomb
            //8
            var legolcsobb = tabla.Where(x => x.utazasMod == comboBox1.SelectedItem).OrderBy(x => x.ar).First();
            listBox2.Items.Clear();
            listBox2.Items.Add($"{legolcsobb.utiCel} - {legolcsobb.indulasDatum}");
            listBox2.Items.Add($"{legolcsobb.napok} nap - {legolcsobb.ar} Ft");

C CAPA KONZ

List <Utazas> tabla = new List<Utazas>();
StreamReader olvaso = new StreamReader("utazasok.txt");
olvaso.ReadLine(); ;
while (!olvaso.EndOfStream)
{
    tabla.Add(new Utazas(olvaso.ReadLine()));
}
olvaso.Close();




//5
int db = 0;
int ossz = 0;
int atlag = 0;

for (int i = 0; i < tabla.Count; i++)
{
    if (tabla[i].utazasMod == "Repülő")
    {
        ossz += tabla[i].ar;
        db++;
    }
}
atlag = ossz / db;
Console.WriteLine($"5. feladat: A repülős utak átlagos ára: {atlag:f2} Ft");






//6

bool vanJarat = false;
for (int i = 0; i < tabla.Count; i++) { 
    if (tabla[i].ar <= 180000 && tabla[i].uticel == "Róma")
    {
        Console.WriteLine($"6. feladat: \n\tIdőpont: {tabla[i].indulas} \n\tNapok száma: {tabla[i].napok} \n\tSzállás: {tabla[i].szallas} ({tabla[i].csillagok}*)\n\tUtazás módja {tabla[i].utazasMod}");
        vanJarat = true;
        break;
    }
}
if (!vanJarat)
{
    Console.WriteLine("Nincs ilyen utazási ajánlat");
}

Console.WriteLine("7. feladat: Statisztika");
//7
int dbRepulo = 0;
int dbBusz = 0;
int dbHajo = 0;
int dbVonat = 0;
int dbAuto = 0;

for (int i = 0; i < tabla.Count; i++)
{
    if (tabla[i].utazasMod == "Repülő")
    {
        dbRepulo++;
    }

    if (tabla[i].utazasMod == "Busz")
    {
        dbBusz++;
    }

    if (tabla[i].utazasMod == "Hajó")
    {
        dbHajo++;
    }

    if (tabla[i].utazasMod == "Vonat")
    {
        dbVonat++;
    }

    if (tabla[i].utazasMod == "Autó")
    {
        dbAuto++;
    }

}
Console.WriteLine($"\tRepülő : {dbRepulo}");
Console.WriteLine($"\tBusz : {dbBusz}");
Console.WriteLine($"\tHajó : {dbHajo}");
Console.WriteLine($"\tVonat : {dbVonat}");
Console.WriteLine($"\tAutó : {dbAuto}");



var Barcelona = tabla.Where(x => x.uticel == "Barcelona").OrderByDescending(x => x.ar);
StreamWriter iro = new StreamWriter("barcelona.txt");

foreach (var b in Barcelona)
{
    iro.WriteLine($"{b.helyszin} ; {b.indulas} ; {b.napok} ; {b.ar}");
}
iro.Close();


Console.ReadKey();




RZNCSI
 
@Entity
@Table(name = "autok")
public class Auto {

  @Id
  @GeneratedValue(strategy = GenerationType.IDENTITY)
  @Column(name = "id")
  private int id;

  @Column(name = "modell")
  private String modell;

  @Column(name = "ulesek_szama")
  private int ulesekSzama;

  @Column(name = "fogyasztas")
  private double fogyasztas;

  @Column(name = "sajat_tomeg")
  private int sajatTomeg;

  @Column(name = "panoramateto")
  private boolean panoramateto;

  @Column(name = "ulesfutes")
  private boolean ulesfutes;

  public Auto() {
  }


  ----
 SCHEAM
 
  CREATE TABLE autok(
    id	INT AUTO_INCREMENT PRIMARY KEY,
    modell	VARCHAR(50)	UNIQUE NOT NULL,
    ulesek_szama INT,
    fogyasztas	DECIMAL(5,2),
    sajat_tomeg	INT,
    panoramateto boolean,
    ulesfutes boolean
);


DATA

INSERT INTO autok (modell,ulesek_szama,fogyasztas,sajat_tomeg,panoramateto,ulesfutes)
VALUES
('VITARA',5,4.7,1600,false,false),
('SWIFT',5,3.85,918,false,false),
('ACROSS',5,5.68,1940,false,true);



-------
repositor

@Repository
public interface AutoRepository extends JpaRepository<Auto,Integer> {
  Auto findByModell(String modell);
}



------
controller

@RestController
@RequestMapping("/api")
public class HomeController {

  private AutoService autoService;

  @Autowired
  public HomeController(AutoService autoService) {
    this.autoService = autoService;
  }

  @GetMapping("/autok")
  public ResponseEntity<?> getAutok(){
    return new ResponseEntity<>(autoService.getAutok(), HttpStatus.OK);
  }

  @PostMapping("/autok")
  public ResponseEntity<?> addauto(@RequestBody Auto auto) {

    if (autoService.isTheModellExist(auto.getModell())){
      return new ResponseEntity<>("A modell már létezik az adatbázisban: "+auto.getModell(), HttpStatus.BAD_REQUEST);
    }

    Auto newAuto=new Auto();
    newAuto.setModell(auto.getModell());
    newAuto.setUlesekSzama(auto.getUlesekSzama());
    newAuto.setFogyasztas(auto.getFogyasztas());
    newAuto.setSajatTomeg(auto.getSajatTomeg());
    newAuto.setPanoramateto(auto.isPanoramateto());
    newAuto.setUlesfutes(auto.isUlesfutes());

    Auto savedAuto=autoService.saveAuto(newAuto);

    Map<String, Integer> id = new HashMap<>();
    id.put("id", savedAuto.getId());

    return new ResponseEntity<>(id, HttpStatus.CREATED);
  }
}
