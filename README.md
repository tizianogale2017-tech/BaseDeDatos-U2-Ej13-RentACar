# BaseDeDatos-U2-Ej13-RentACar
Base de Datos - Unidad 2 - Ejercicio 13

Consigna

Modelar la operación de una empresa internacional de alquiler de autos: sucursales en aeropuertos y centros urbanos, flota de vehículos asignados a una sucursal, clientes con licencia vigente, contratos de alquiler con retiro y devolución en sucursales posiblemente distintas, y siniestros ocurridos durante la vigencia del contrato.

Lógica

SUCURSAL 1:N VEHICULO. El enunciado dice que el vehículo está asignado a una sucursal "en un momento dado": es una relación 1:N con la sucursal actual, que se actualiza cuando la unidad se reubica. No es N:M: guardar el historial de ubicaciones sería otro requerimiento y exigiría una entidad de movimientos con fechas. Lo que sí queda registrado indirectamente es el recorrido entre sucursales, porque cada contrato guarda de dónde salió y a dónde volvió.

Las dos sucursales del contrato son dos relaciones distintas, no una sola. Acá está el punto 3 del desarrollo: SUCURSAL participa dos veces en CONTRATO con roles diferentes — retira_en y devuelve_en — y cada una genera su propia FK (Id_Suc_Retiro e Id_Suc_Devolucion). Es el mismo mecanismo de roles que en una autorrelación, solo que acá los dos extremos son entidades distintas. Una sola relación no serviría: no se podría distinguir el origen del destino, y modelarlo como N:M sería incorrecto porque un contrato tiene exactamente una sucursal de cada tipo. El caso normal es que ambas coincidan, pero el modelo tiene que soportar el alquiler one-way.

CONTRATO como entidad, no como relación N:M entre cliente y vehículo. Tiene identidad propia, atributos abundantes (fechas previstas y reales, importe) y se relaciona además con dos sucursales y con los siniestros. Convertirlo en un simple rombo dejaría sin lugar a los siniestros.

Devolución prevista y devolución real, separadas. Son dos atributos distintos porque la real puede no existir todavía (contrato en curso, queda en NULL) o diferir de la prevista, que es justamente lo que permite calcular recargos por demora.

SINIESTRO colgado del CONTRATO y no del VEHICULO. El contrato ya identifica al vehículo, al cliente y al período, así que colgando el siniestro del contrato queda determinado quién era el responsable al momento del incidente, que es el dato que el negocio necesita para cobrar la franquicia. Si colgara del vehículo habría que cruzar fechas contra los contratos para deducir el responsable. Contrapartida a tener en cuenta: los siniestros ocurridos fuera de un alquiler (maniobras en playa, traslados entre sucursales) no entrarían en este modelo; para cubrirlos habría que agregar una FK opcional al vehículo.

ASEGURADORA como entidad propia. Es un dato repetido entre muchos siniestros (nombre, contacto, póliza); dejarlo como texto dentro del siniestro duplicaría información y no permitiría consultar todos los casos de una aseguradora.

Restricciones de integridad a considerar: un vehículo no puede tener dos contratos con períodos solapados; la licencia del cliente debe estar vigente a la fecha de retiro; la devolución real no puede ser anterior al retiro; y el kilometraje del vehículo solo debería aumentar al cerrar cada contrato.

Resultado
Resultado
<img width="4650" height="2764" alt="BaseDeDatos-U2-Ej13-RentACar" src="https://github.com/user-attachments/assets/523b996f-85b5-4e8d-a62a-a832774433e4" />
