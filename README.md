# Toth-
Dental clinic
<!-- نفس الهيكل السابق مع تعديل التخزين -->
<script>
    // مفتاح التشفير (غيّره إلى مفتاح طويل ومعقد ولا تخبر به أحداً)
    const ENCRYPTION_KEY = "عيادة@سما#2025$$$key123!";

    function encryptData(data) {
        return CryptoJS.AES.encrypt(JSON.stringify(data), ENCRYPTION_KEY).toString();
    }

    function decryptData(ciphertext) {
        try {
            const bytes = CryptoJS.AES.decrypt(ciphertext, ENCRYPTION_KEY);
            return JSON.parse(bytes.toString(CryptoJS.enc.Utf8));
        } catch(e) { return []; }
    }

    // تحميل الحجوزات المشفرة
    let bookings = [];
    const encryptedBookings = localStorage.getItem('clinicBookingsEncrypted');
    if (encryptedBookings) {
        bookings = decryptData(encryptedBookings);
    } else {
        bookings = [];
    }

    // عند إضافة حجز جديد
    document.getElementById('bookingForm').addEventListener('submit', function(e) {
        e.preventDefault();
        const newBooking = {
            id: Date.now(),
            name: document.getElementById('name').value,
            phone: document.getElementById('phone').value,
            email: document.getElementById('email').value,
            service: document.getElementById('service').value,
            date: document.getElementById('date').value,
            timestamp: new Date().toLocaleString('ar-EG')
        };
        bookings.push(newBooking);
        // تخزين مشفر
        localStorage.setItem('clinicBookingsEncrypted', encryptData(bookings));
        document.getElementById('formMessage').innerHTML = '<p style="color:green;">✓ تم الحجز بنجاح</p>';
        document.getElementById('bookingForm').reset();
        setTimeout(() => { document.getElementById('formMessage').innerHTML = ''; }, 3000);
    });
</script>
